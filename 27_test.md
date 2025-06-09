```matlab
clear;
close all;
clc;

% ファイル名は適宜変更してください
inputFile    = 'input_test.csv';        % 入力テストパターンCSV
expectedFile = 'expected_test.csv';     % 期待値パターンCSV

%─── CSV 読み込み ─────────────────────────
T_in  = readtable(inputFile);
T_exp = readtable(expectedFile);

%─── .mat ファイルとして保存 ───────────────────
% テーブルごと丸ごと保存
save('input_test.mat',    'T_in');
save('expected_test.mat', 'T_exp');

%（もし個別変数でも残したい場合は以下のように）
t_in   = T_in.time;
x_in   = T_in.x;   y_in   = T_in.y;   z_in   = T_in.z;

t_exp  = T_exp.time;
x_exp  = T_exp.exp_x;  y_exp  = T_exp.exp_y;  z_exp  = T_exp.exp_z;

save('input_vars.mat',    't_in','x_in','y_in','z_in');
save('expected_vars.mat', 't_exp','x_exp','y_exp','z_exp');

%─── シミュレーション用 ───────────────────────────
% 例：すでに t_in, x_in, y_in, z_in がある場合
x_ts = timeseries(x_in, t_in);
y_ts = timeseries(y_in, t_in);
z_ts = timeseries(z_in, t_in);

%─── プロット ───────────────────────────
figure('Name','テストパターン別プロット','NumberTitle','off');

% --- 入力データ x_in ---
subplot(6,1,1);
plot(t_in, x_in, 'b-', 'LineWidth',1.5);
ylabel('x_{in}');
title('入力データ x');
grid on;

% --- 入力データ y_in ---
subplot(6,1,2);
plot(t_in, y_in, 'b-', 'LineWidth',1.5);
ylabel('y_{in}');
title('入力データ y');
grid on;

% --- 入力データ z_in ---
subplot(6,1,3);
plot(t_in, z_in, 'b-', 'LineWidth',1.5);
ylabel('z_{in}');
title('入力データ z');
grid on;

% --- 期待値データ exp_x ---
subplot(6,1,4);
plot(t_exp, x_exp, 'r--', 'LineWidth',1.5);
ylabel('x_{exp}');
title('期待値データ x');
grid on;

% --- 期待値データ exp_y ---
subplot(6,1,5);
plot(t_exp, y_exp, 'r--', 'LineWidth',1.5);
ylabel('y_{exp}');
title('期待値データ y');
grid on;

% --- 期待値データ exp_z ---
subplot(6,1,6);
plot(t_exp, z_exp, 'r--', 'LineWidth',1.5);
xlabel('Time [s]');
ylabel('z_{exp}');
title('期待値データ z');
grid on;

```

```matlab
%% run_sim_model.m
%  1) モデル名を設定
modelName = 'test_model';   % ←実際の .slx／.mdl のファイル名に合わせてください

%  2) モデルをロード（初回のみ必要）
load_system(modelName);

%  3) シミュレーション終了時刻を設定
%     (例：入力データの最終時刻を StopTime に)
StopTime = num2str(max(t_in));  
set_param(modelName, 'StopTime', StopTime);

% ────────── 固定ステップソルバーの設定 ──────────
% ソルバータイプを「固定ステップ」に
set_param(modelName, 'SolverType', 'Fixed-step');
% ソルバーを「ode4（4次ルンゲクッタ）」に（離散モデルなら 'FixedStepDiscrete' でもOK）
set_param(modelName, 'Solver', 'ode4');
% 固定ステップ幅を 0.01 秒に
set_param(modelName, 'FixedStep', '0.01');
% ─────────────────────────────────────────────

%  4) ワークスペースの時系列データを Inport 経由で流し込む場合
% （あらかじめ input.time, input.signals.values, input.signals.dimensions を
%  準備してある想定）
%  modelConfiguration で Inport からこの構造体を読み込む設定にしておくこと
%  → Simulation > Model Configuration Parameters > Data Import/Export > Input: input

%  5) シミュレーション実行
simOut = sim( ...
    modelName, ...
    'SrcWorkspace','current', ...     % ワークスペース変数を参照
    'SaveOutput','on', ...            % Outport を構造体で取得
    'OutputSaveName','yout', ...      % 出力信号構造体の変数名
    'SaveTime','on', ...              % 時間ベクトルも保存
    'TimeSaveName','tout' ...         % 時間ベクトルの変数名
);

%  6) 出力結果を取り出し
%    Outport ブロックで名前を付けた場合は simOut.get('<varName>') でも取得可能ですが、
%    ここでは構造体 yout.signals を使う例を示します。
t_out = simOut.tout;                  % シミュレーション時間ベクトル
% ySignals = simOut.yout.signals;      % signals: array of structures

%  モデルで Outport を 1:x_out, 2:y_out, 3:z_out の順に配置している想定
% x_out = ySignals(1).values;   
% y_out = ySignals(2).values;
% z_out = ySignals(3).values;

x_out = simOut.x_out;
y_out = simOut.y_out;
z_out = simOut.z_out;

%  7) (必要なら) .mat へ保存
save('sim_result.mat','tout','x_out','y_out','z_out');

%  8) 確認用メッセージ
fprintf('モデル「%s」をシミュレーションしました。結果を x_out, y_out, z_out として取得しました。\n', modelName);

```

```matlab
% 比較＆プロットスクリプト
% 前提：ワークスペースに以下の変数があるものとします
%   t_out  – シミュレーション出力時刻ベクトル
%   x_out  – シミュレーション出力 x 値ベクトル
%   y_out  – シミュレーション出力 y 値ベクトル
%   z_out  – シミュレーション出力 z 値ベクトル
%   t_exp  – 期待値（exp_x, exp_y, exp_z）の時刻ベクトル
%   x_exp  – 期待値 x 値ベクトル
%   y_exp  – 期待値 y 値ベクトル
%   z_exp  – 期待値 z 値ベクトル

tol = 0.001;  % 許容誤差

% 期待値をシミュレーション時間に合わせて補間
% x_exp_i = interp1(t_exp, x_exp, t_out);
% y_exp_i = interp1(t_exp, y_exp, t_out);
% z_exp_i = interp1(t_exp, z_exp, t_out);

% 誤差計算
% err_x = abs(x_out - x_exp_i);
% err_y = abs(y_out - y_exp_i);
% err_z = abs(z_out - z_exp_i);

x_out.data

err_x = abs(x_out.data - x_exp);
err_y = abs(y_out.data - y_exp);
err_z = abs(z_out.data - z_exp);

% 判定
pass_x = all(err_x <= tol);
pass_y = all(err_y <= tol);
pass_z = all(err_z <= tol);

fprintf('X の比較: %s\n', ternary(pass_x, 'PASS', 'FAIL'));
fprintf('Y の比較: %s\n', ternary(pass_y, 'PASS', 'FAIL'));
fprintf('Z の比較: %s\n', ternary(pass_z, 'PASS', 'FAIL'));

% % 最大誤差を求め
% max_err_x = max(err_x);
% max_err_y = max(err_y);
% max_err_z = max(err_z);
% 
% % 許容内なら 0.000、超えたらその値を表示
% if max_err_x <= tol
%     disp_err_x = 0.000;
% else
%     disp_err_x = max_err_x;
% end
% 
% if max_err_y <= tol
%     disp_err_y = 0.000;
% else
%     disp_err_y = max_err_y;
% end
% 
% if max_err_z <= tol
%     disp_err_z = 0.000;
% else
%     disp_err_z = max_err_z;
% end
% 
% % 判定結果を表示
% if max_err_x <= tol
%     fprintf('X の比較: PASS (max err = %.3f)\n', disp_err_x);
% else
%     fprintf('X の比較: FAIL (max err = %.3f)\n', disp_err_x);
% end
% 
% if max_err_y <= tol
%     fprintf('Y の比較: PASS (max err = %.3f)\n', disp_err_y);
% else
%     fprintf('Y の比較: FAIL (max err = %.3f)\n', disp_err_y);
% end
% 
% if max_err_z <= tol
%     fprintf('Z の比較: PASS (max err = %.3f)\n', disp_err_z);
% else
%     fprintf('Z の比較: FAIL (max err = %.3f)\n', disp_err_z);
% end

% 各軸の最大誤差とそのインデックス
[max_err_x, idx_x] = max(err_x);
[max_err_y, idx_y] = max(err_y);
[max_err_z, idx_z] = max(err_z);

% 表示用誤差（許容内なら 0.000）
if max_err_x <= tol, disp_err_x = 0.000; else disp_err_x = max_err_x; end
if max_err_y <= tol, disp_err_y = 0.000; else disp_err_y = max_err_y; end
if max_err_z <= tol, disp_err_z = 0.000; else disp_err_z = max_err_z; end

% コマンドウィンドウ出力
if max_err_x <= tol
    fprintf('X の比較: PASS (max err = %.3f)\n', disp_err_x);
else
    fprintf('X の比較: FAIL (max err = %.3f)\n', disp_err_x);
end
if max_err_y <= tol
    fprintf('Y の比較: PASS (max err = %.3f)\n', disp_err_y);
else
    fprintf('Y の比較: FAIL (max err = %.3f)\n', disp_err_y);
end
if max_err_z <= tol
    fprintf('Z の比較: PASS (max err = %.3f)\n', disp_err_z);
else
    fprintf('Z の比較: FAIL (max err = %.3f)\n', disp_err_z);
end

% プロット
figure('Name','Simulink vs Expected (with Max Error)','NumberTitle','off');
err_line = tol;

% --- X 比較 ---
subplot(3,1,1);
plot(t_out, x_out.data, 'b-', 'LineWidth',1.5); hold on;
plot(t_out, x_exp_i,    'r--','LineWidth',1.5);
yline( err_line,'k:'); yline(-err_line,'k:');
% 最大誤差マーカー＆数値
tx = t_out(idx_x);
px = x_out.data(idx_x);
plot(tx, px, 'ko', 'MarkerFaceColor','y');
text(tx, px, sprintf('  %.3f', disp_err_x), 'VerticalAlignment','bottom');
hold off;
title('X: Simulink 出力 vs 期待値');
xlabel('Time [s]'); ylabel('x');
legend('SimOut','Expected','Tol','Location','best');
grid on;

% --- Y 比較 ---
subplot(3,1,2);
plot(t_out, y_out.data, 'b-', 'LineWidth',1.5); hold on;
plot(t_out, y_exp_i,    'r--','LineWidth',1.5);
yline( err_line,'k:'); yline(-err_line,'k:');
% 最大誤差マーカー＆数値
ty = t_out(idx_y);
py = y_out.data(idx_y);
plot(ty, py, 'ko', 'MarkerFaceColor','y');
text(ty, py, sprintf('  %.3f', disp_err_y), 'VerticalAlignment','bottom');
hold off;
title('Y: Simulink 出力 vs 期待値');
xlabel('Time [s]'); ylabel('y');
legend('SimOut','Expected','Tol','Location','best');
grid on;

% --- Z 比比較 ---
subplot(3,1,3);
plot(t_out, z_out.data, 'b-', 'LineWidth',1.5); hold on;
plot(t_out, z_exp_i,    'r--','LineWidth',1.5);
yline( err_line,'k:'); yline(-err_line,'k:');
% 最大誤差マーカー＆数値
tz = t_out(idx_z);
pz = z_out.data(idx_z);
plot(tz, pz, 'ko', 'MarkerFaceColor','y');
text(tz, pz, sprintf('  %.3f', disp_err_z), 'VerticalAlignment','bottom');
hold off;
title('Z: Simulink 出力 vs 期待値');
xlabel('Time [s]'); ylabel('z');
legend('SimOut','Expected','Tol','Location','best');
grid on;

% % プロット
% figure('Name','Simulink vs Expected','NumberTitle','off');
% 
% % X 比較
% subplot(3,1,1);
% plot(t_out, x_out.data, 'b-', 'LineWidth',1.5); hold on;
% plot(t_out, x_exp, 'r--', 'LineWidth',1.5);
% yline(tol, 'k:', '許容誤差'); yline(-tol, 'k:');
% hold off;
% title('X: Simulink 出力 vs 期待値');
% xlabel('Time [s]'); ylabel('x');
% legend('SimOut','Expected','Location','best');
% grid on;
% 
% % Y 比較
% subplot(3,1,2);
% plot(t_out, y_out.data, 'b-', 'LineWidth',1.5); hold on;
% plot(t_out, y_exp, 'r--', 'LineWidth',1.5);
% yline(tol, 'k:'); yline(-tol, 'k:');
% hold off;
% title('Y: Simulink 出力 vs 期待値');
% xlabel('Time [s]'); ylabel('y');
% legend('SimOut','Expected','Location','best');
% grid on;
% 
% % Z 比較
% subplot(3,1,3);
% plot(t_out, z_out.data, 'b-', 'LineWidth',1.5); hold on;
% plot(t_out, z_exp, 'r--', 'LineWidth',1.5);
% yline(tol, 'k:'); yline(-tol, 'k:');
% hold off;
% title('Z: Simulink 出力 vs 期待値');
% xlabel('Time [s]'); ylabel('z');
% legend('SimOut','Expected','Location','best');
% grid on;


% 補助関数：Ternary operator
function out = ternary(cond, trueVal, falseVal)
    if cond
        out = trueVal;
    else
        out = falseVal;
    end
end

```

