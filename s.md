```matlab
% ── runCsv2Asc.m ───────────────────────────────────────────────
% runCsv2Asc  example_input.csv を自動で変換して output.asc を生成
%
%   このスクリプトを実行するだけで CSV → ASC 変換が行われます。

% 入出力ファイル名
csvInput  = 'example_input.csv';
ascOutput = 'output1.asc';

% 存在チェック
if ~isfile(csvInput)
    error('CSV file "%s" が見つかりません。', csvInput);
end

% 変換関数を呼び出し
csv2asc_custom(csvInput, ascOutput);

============================

function csv2asc_custom(csvFile, ascFile)
% csv2asc_custom  メタ情報付きCSV → ASC フォーマットに変換
%   csvFile : 入力CSVファイル名
%   ascFile : 出力ASCファイル名

    %--- 入力CSVオープン ---
    fid_in = fopen(csvFile,'r');
    if fid_in < 0
        error('Cannot open input CSV: %s', csvFile);
    end

    %--- 先頭4行：メタ情報を文字列として取得＆charに変換 ---
    line1 = convertStringsToChars( fgetl(fid_in) )  % varType
    line2 = convertStringsToChars( fgetl(fid_in) );  % sign
    line3 = convertStringsToChars( fgetl(fid_in) )  % bus
    line4 = convertStringsToChars( fgetl(fid_in) )  % varName
    fgetl(fid_in);  % "time,x,y,z" ヘッダ行をスキップ
    
        %--- strsplitでカンマ区切り、先頭空要素を除去 ---
    meta1 = strsplit(line1, ',');  varTypeList = meta1(2:end);
    meta2 = strsplit(line2, ','); signList    = meta2(2:end);
    meta3 = strsplit(line3, ','); busList     = meta3(2:end);
    meta4 = strsplit(line4, ','); nameList    = meta4(2:end);
    
    %--- 出力ASCファイルオープン ---
    fid_out = fopen(ascFile,'w');
    if fid_out < 0
        fclose(fid_in);
        error('Cannot open output ASC: %s', ascFile);
    end

    %--- データ部を1行ずつ読み込み変換 ---
    while true
        tline = fgetl(fid_in);
        if ~ischar(tline), break; end       % EOF
        if isempty(strtrim(tline)), continue; end  % 空行スキップ

        parts = strsplit(tline,',');         % {'0.0','0.1','0.1','0.1'}
        sec   = str2double(parts{1});        % 秒

        for j = 2:numel(parts)
            val   = str2double(parts{j});    % 値
            vtype = varTypeList{j-1};        % 文字列型
            sgn   = signList{j-1};
            bus   = busList{j-1};
            name  = nameList{j-1};

            % 書式: [sec] SV: [varType] 0 [sign] ::[bus]::[varName] = [value]
            fprintf(fid_out, '%.3f SV: %s 0 %s ::%s::%s = %.6f\n', ...
                sec, vtype, sgn, bus, name, val);
        end
    end

    %--- クローズ ---
    fclose(fid_in);
    fclose(fid_out);
    fprintf('Wrote ASC to %s\n', ascFile);
end

```

