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

```matlab
%% dynamicPlotTestPatterns.m
% CSV を読み込んで .mat 保存、timeseries 作成、変数名で動的プロット
% 期待値データの処理・プロットはユーザー選択で ON/OFF を切り替え
clear; close all; clc;

%% 1) 入力データの読み込み・処理
[inputTime, inputValues, inputVarNames] = loadCsvData('input_test.csv');
save('input_test.mat', 'inputTime','inputValues','inputVarNames');
createTimeseriesObjects(inputTime, inputValues, inputVarNames);
plotTimeSeries(inputTime, inputValues, inputVarNames, '入力テストパターン', 'b-');

%% 2) 期待値データを処理するかユーザー選択
answ = input('期待値データを処理・プロットしますか？ (Y/N) [Y]: ','s');
if isempty(answ), answ = 'Y'; end
doExpected = strcmpi(answ,'Y');

if doExpected
    %% 2-1) 期待値データの読み込み
    [expTime, expValues, expVarNames] = loadCsvData('expected_test.csv');
    %% 2-2) .mat 保存
    save('expected_test.mat', 'expTime','expValues','expVarNames');
    %% 2-3) timeseries オブジェクト生成
    createTimeseriesObjects(expTime, expValues, expVarNames);
    %% 2-4) 期待値データのプロット
    plotTimeSeries(expTime, expValues, expVarNames, '期待値テストパターン', 'r--');
else
    fprintf('期待値データの処理・プロットをスキップしました。\n');
end

%% ─── ローカル関数 ─────────────────────────────────────────────

function [timeVec, dataMat, varNames] = loadCsvData(csvFile)
% loadCsvData  CSVファイルを読み込み、時刻ベクトル, データ行列, 変数名を取得
    tbl      = readtable(csvFile, 'TextType','string');
    timeVec  = tbl{:,1};            % 1列目 = time
    dataMat  = tbl{:,2:end};        % 以降の列がデータ
    varNames = tbl.Properties.VariableNames(2:end);
end

function createTimeseriesObjects(timeVec, dataMat, varNames)
% createTimeseriesObjects  MATLAB ワークスペースに timeseries オブジェクトを作成
    for i = 1:numel(varNames)
        tsName = [varNames{i} '_ts'];
        assignin('base', tsName, timeseries(dataMat(:,i), timeVec));
    end
end

function plotTimeSeries(timeVec, dataMat, varNames, figTitle, lineSpec)
% plotTimeSeries  変数名をラベルに、自動で縦 n 行プロット
    n = numel(varNames);
    figure('Name',figTitle,'NumberTitle','off');
    for k = 1:n
        subplot(n,1,k);
        plot(timeVec, dataMat(:,k), lineSpec, 'LineWidth',1.5);
        ylabel(varNames{k}, 'Interpreter','none');
        title([figTitle ' - ' varNames{k}], 'Interpreter','none');
        grid on;
        if k==n
            xlabel('Time [s]');
        end
    end
end
```

```matlab
%% dynamicPlotTestPatterns.m
% CSV を読み込んで .mat 保存、timeseries 作成、変数名で動的プロット
% - 期待値データの処理・プロットはユーザー選択
% - 入力データの描画変数もユーザー選択可能
clear; close all; clc;

%% 1) 入力データ読み込み
[inputTime, inputValues, inputVarNames] = loadCsvData('input_test.csv');
save('input_test.mat', 'inputTime','inputValues','inputVarNames');

%% 2) ユーザーに入力データの描画変数を選択させる
fprintf('--- 入力変数一覧 ---\n');
for i = 1:numel(inputVarNames)
    fprintf('  %2d: %s\n', i, inputVarNames{i});
end
sel = input('描画したい変数の番号を [1 3 5] のように入力（Enter で全選択）: ');
if isempty(sel)
    sel = 1:numel(inputVarNames);
end
sel = sel(sel>=1 & sel<=numel(inputVarNames));  % 範囲チェック
selNames  = inputVarNames(sel);
selValues = inputValues(:,sel);

%% 3) Simulink 用 timeseries オブジェクト生成（必要なら）
createTimeseriesObjects(inputTime,    inputValues,    inputVarNames);

%% 4) 選択された入力変数をプロット
plotTimeSeries(inputTime, selValues, selNames, '入力テストパターン', 'b-');

%% 5) 期待値データの処理・プロット選択
answ = input('期待値データを処理・プロットしますか？ (Y/N) [Y]: ','s');
if isempty(answ), answ = 'Y'; end
if strcmpi(answ,'Y')
    [expTime, expValues, expVarNames] = loadCsvData('expected_test.csv');
    save('expected_test.mat', 'expTime','expValues','expVarNames');
    createTimeseriesObjects(expTime, expValues, expVarNames);
    plotTimeSeries(expTime, expValues, expVarNames, '期待値テストパターン', 'r--');
else
    fprintf('期待値データの処理・プロットをスキップしました。\n');
end


%% ─── ローカル関数 ─────────────────────────────────────────────

function [timeVec, dataMat, varNames] = loadCsvData(csvFile)
% loadCsvData  CSV を読み込み、時刻ベクトル、データ行列、変数名を取得
    tbl      = readtable(csvFile, 'TextType','string');
    timeVec  = tbl{:,1};            % 1列目 = time
    dataMat  = tbl{:,2:end};        % 以降の列がデータ
    varNames = tbl.Properties.VariableNames(2:end);
end

function createTimeseriesObjects(timeVec, dataMat, varNames)
% createTimeseriesObjects  MATLAB ワークスペースに timeseries オブジェクトを作成
    for i = 1:numel(varNames)
        tsName = [varNames{i} '_ts'];
        assignin('base', tsName, timeseries(dataMat(:,i), timeVec));
    end
end

function plotTimeSeries(timeVec, dataMat, varNames, figTitle, lineSpec)
% plotTimeSeries  変数名をラベルに、自動で縦 n 行プロット
    n = numel(varNames);
    figure('Name', figTitle, 'NumberTitle','off');
    for k = 1:n
        subplot(n,1,k);
        plot(timeVec, dataMat(:,k), lineSpec, 'LineWidth',1.5);
        ylabel(varNames{k}, 'Interpreter','none');
        title([figTitle ' - ' varNames{k}], 'Interpreter','none');
        grid on;
        if k==n
            xlabel('Time [s]');
        end
    end
end
```

---
25.10.5

```matlab
clear;
close all;
clc;

% plot_io_grouped_csv('csv_data/input_test10.csv');
% plot_io_grouped_csv2('csv_data/input_test10.csv');
plot_io_grouped_csv2('testdata/test01.csv');

function S = plot_io_grouped_csv2(csvPath)
% plot_io_grouped_csv2 (R2019a対応)
% 1行目: 見出し（"入力" 開始列 と "出力" 開始列。他セルは空欄）
% 2行目: 列名（例: time,x,y,z,w,v）
% 3行目～: データ
%
% 追加仕様:
%   - 入力を MAT 保存:  testdata/<csv名>_input.mat  （列名そのまま）
%   - 出力を MAT 保存:  testdata/<csv名>_output.mat （列名そのまま）
%   - MAT 内には常に 'time' も保存

% ===== 固定スタイル =====
FIG_POS   = [100 100 400 600];
LW        = 1.6;
FONTSIZE  = 11;
GRID_ON   = true;
INPUT_TAG = '入力';
OUTPUT_TAG= '出力';
TIMENAME  = 'time';
SAVE_DIR_IMG  = 'testdata';   % （CSVと同フォルダ内にある時のみ保存）
SAVE_DIR_MAT  = 'testdata';  % （カレント直下）なければ作成

assert(exist(csvPath,'file')==2, 'ファイルが見つかりません: %s', csvPath);

% ===== 読み込み（#行/空行は無視） =====
txt = fileread(csvPath); txt = rm_bom_if_any(txt);
lines = regexp(txt, '\r\n|\n|\r', 'split');
keep = true(1,numel(lines));
for i=1:numel(lines)
    t = strtrim(lines{i});
    if isempty(t) || (~isempty(t) && t(1)=='#'), keep(i)=false; end
end
lines = lines(keep);
assert(numel(lines) >= 3, 'CSVは少なくとも3行（見出し/列名/データ）が必要です。');

% ===== 見出し/列名 =====
roleRow = split_preserve_empty(lines{1});
nameRow = split_preserve_empty(lines{2});
nc = max(numel(roleRow), numel(nameRow));
if numel(roleRow) < nc, roleRow(end+1:nc) = {''}; end
if numel(nameRow) < nc, nameRow(end+1:nc) = {''}; end

idxIn  = find(strcmp(roleRow, INPUT_TAG), 1, 'first');
idxOut = find(strcmp(roleRow, OUTPUT_TAG), 1, 'first');
assert(~isempty(idxIn),  '1行目に「%s」が見つかりません。', INPUT_TAG);
assert(~isempty(idxOut), '1行目に「%s」が見つかりません。', OUTPUT_TAG);
assert(idxOut > idxIn, '「%s」は「%s」より右側に配置してください。', OUTPUT_TAG, INPUT_TAG);

% ===== データを table で読む =====
tmp = [tempname,'.csv'];
fid = fopen(tmp,'w');
fprintf(fid, '%s\n', join_with_commas(nameRow));
for i=3:numel(lines), fprintf(fid, '%s\n', lines{i}); end
fclose(fid);
T = readtable(tmp); delete(tmp);

varNames = T.Properties.VariableNames;
timeIdx = find(strcmpi(varNames, TIMENAME), 1, 'first');
assert(~isempty(timeIdx), '2行目の列名に "%s" が必要です。', TIMENAME);

% ===== 入出力列の決定（位置ベース, timeは除外） =====
ncols = numel(varNames);
inputRange  = max(idxIn,1)  : max(idxOut-1,0);
outputRange = max(idxOut,1) : nc;
inputRange  = inputRange(inputRange  >=1 & inputRange  <= ncols);
outputRange = outputRange(outputRange>=1 & outputRange<= ncols);
inputRange(inputRange==timeIdx)   = [];
outputRange(outputRange==timeIdx) = [];

% ===== 構造体へ格納 =====
S = struct();
S.time = T.(varNames{timeIdx});
S.input = table();
for k = inputRange,  nm = varNames{k}; S.input.(nm)  = T.(nm); end
S.output = table();
for k = outputRange, nm = varNames{k}; S.output.(nm) = T.(nm); end
S.info = struct('csv',csvPath, ...
    'timeColumn',TIMENAME, ...
    'inputVars',{S.input.Properties.VariableNames}, ...
    'outputVars',{S.output.Properties.VariableNames});

% ===== 描画（縦一列：入力→出力、青・赤） =====
nIn  = width(S.input); nOut = width(S.output); nTot = nIn + nOut;
[csvDir, base, ~] = fileparts(csvPath);
fig = figure('Color','w','Position',FIG_POS,'Name','IO Timeseries (Vertical)');
set(fig,'DefaultAxesFontSize',FONTSIZE);

idxPlot = 1;
for i=1:nIn
    ax = subplot(nTot,1,idxPlot);
    plot(S.time, S.input{:,i}, 'LineWidth', LW, 'Color', 'blue');
    axis tight; if GRID_ON, grid on; end
    ylabel(S.input.Properties.VariableNames{i}, 'Interpreter','none');
    if idxPlot < nTot, set(ax,'XTickLabel',[]); else, xlabel('Time [s]'); end
    idxPlot = idxPlot+1;
end
for i=1:nOut
    ax = subplot(nTot,1,idxPlot);
    plot(S.time, S.output{:,i}, 'LineWidth', LW, 'Color', 'red');
    axis tight; if GRID_ON, grid on; end
    ylabel(S.output.Properties.VariableNames{i}, 'Interpreter','none');
    if idxPlot < nTot, set(ax,'XTickLabel',[]); else, xlabel('Time [s]'); end
    idxPlot = idxPlot+1;
end

% ===== 画像保存（CSVと同フォルダ内 results/ がある場合のみ） =====
if exist(SAVE_DIR_IMG,'dir') == 7
    [~, base, ~] = fileparts(csvPath);
    outPng = fullfile(SAVE_DIR_IMG, [base, '_io.png']);
    print(fig, outPng, '-dpng', '-r200');
end

% ======== MAT 保存（testdata/ に input と output を個別保存） =========
% 保存先ディレクトリ（カレント直下）を用意
if exist(SAVE_DIR_MAT,'dir') ~= 7, mkdir(SAVE_DIR_MAT); end

% --- 入力の保存 ---
inputNames = S.input.Properties.VariableNames;
invalidMask = ~cellfun(@isvarname, inputNames);
if any(invalidMask)
    error('MAT変数名として無効な入力列名があります: %s', strjoin(inputNames(invalidMask), ', '));
end
if numel(unique(inputNames)) ~= numel(inputNames)
    error('入力列名が重複しています。CSVの列名を一意にしてください。');
end
toSaveIn = struct(); toSaveIn.(TIMENAME) = S.time;
for i=1:numel(inputNames)
    nm = inputNames{i};
    toSaveIn.(nm) = S.input{:,i};
end
save(fullfile(SAVE_DIR_MAT, [base, '_input.mat']), '-struct', 'toSaveIn');

% --- 出力の保存 ---
outputNames = S.output.Properties.VariableNames;
if ~isempty(outputNames)
    invalidMask = ~cellfun(@isvarname, outputNames);
    if any(invalidMask)
        error('MAT変数名として無効な出力列名があります: %s', strjoin(outputNames(invalidMask), ', '));
    end
    if numel(unique(outputNames)) ~= numel(outputNames)
        error('出力列名が重複しています。CSVの列名を一意にしてください。');
    end
    toSaveOut = struct(); toSaveOut.(TIMENAME) = S.time;
    for i=1:numel(outputNames)
        nm = outputNames{i};
        toSaveOut.(nm) = S.output{:,i};
    end
    save(fullfile(SAVE_DIR_MAT, [base, '_output.mat']), '-struct', 'toSaveOut');
end

end

% ---------- ヘルパ ----------
function out = rm_bom_if_any(in)
if isempty(in), out = in; return; end
bom = char([239 187 191]); % EF BB BF
if strncmp(in, bom, 1), out = in(4:end); else, out = in; end
end
function cells = split_preserve_empty(line)
cells = strsplit(line, ',', 'CollapseDelimiters', false);
for i=1:numel(cells), cells{i} = strtrim(cells{i}); end
end
function s = join_with_commas(cells)
if isempty(cells), s = ''; return; end
s = cells{1};
for i=2:numel(cells), s = [s, ',', cells{i}]; end %#ok<AGROW>
end

```

