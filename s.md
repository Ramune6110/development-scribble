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

```matlab
function convertOld2NewAsc(oldAscFile, excelFile, newAscFile)
% convertOld2NewAsc  旧型ASC → 新型ASCに一括変換
% 
% oldAscFile : 旧型ASCファイル名（例: 'old.asc'）
% excelFile  : 変数一覧Excelファイル名（例: 'vars.xlsx'）
% newAscFile : 出力先新型ASCファイル名（例: 'new.asc'）
%
% 旧型ASCフォーマット:
%   0.000 x := 0.100
%
% 変数一覧Excelのフォーマット（1行目ヘッダー／4列固定）:
%   環境変数名 | マクロパス | 変数型 | 符号
%   x           | G5M        | 1     | 1
%   y           | G5M        | 2     | 1
%
% 新型ASCフォーマット:
%   0.000 SV: 1 0 1 ::G5M::x = 0.100000

    %--------------------------------------------------------------------------
    % 1) 変数一覧Excelファイルをセル配列で読み込む
    raw = readcell(excelFile);
    % raw(1,:) はヘッダー行、raw(2:end,:) がデータ
    envNames   = raw(2:end, 1);        % { 'x'; 'y'; ... }
    macroPaths = raw(2:end, 2);        % { 'G5M'; 'G5M'; ... }
    varTypes   = cell2mat(raw(2:end, 3)); % [1; 2; ...]
    signs      = cell2mat(raw(2:end, 4)); % [1; 1; ...]
    %--------------------------------------------------------------------------

    %--------------------------------------------------------------------------
    % 2) 旧型ASCファイルをオープン
    fidIn = fopen(oldAscFile, 'r');
    if fidIn < 0
        error('Cannot open old ASC file: %s', oldAscFile);
    end
    fidOut = fopen(newAscFile, 'w');
    if fidOut < 0
        fclose(fidIn);
        error('Cannot open new ASC file: %s', newAscFile);
    end
    %--------------------------------------------------------------------------

    %--------------------------------------------------------------------------
    % 3) 旧型ASCを一行ずつ読み込み→新型フォーマットで出力
    %    旧型行例: "0.100 x := 0.200"
    expr = '^\s*(?<sec>[-+0-9.eE]+)\s+(?<name>\w+)\s*:=\s*(?<val>[-+0-9.eE]+)';
    while true
        line = fgetl(fidIn);
        if ~ischar(line), break; end  % EOF
        tok = regexp(line, expr, 'names');
        if isempty(tok), continue; end

        sec  = str2double(tok.sec);
        name = tok.name;
        val  = str2double(tok.val);

        % 変数一覧からインデックスを探す
        idx = find(strcmp(envNames, name), 1);
        if isempty(idx)
            warning('Variable "%s" not found in %s', name, excelFile);
            continue;
        end

        % 対応するマクロパス等を取得
        vtype = varTypes(idx);
        sgn   = signs(idx);
        macro = macroPaths{idx};
        env   = envNames{idx};

        % 新型フォーマットで書き込み
        fprintf(fidOut, '%.3f SV: %d 0 %d ::%s::%s = %.6f\n', ...
            sec, vtype, sgn, macro, env, val);
    end
    %--------------------------------------------------------------------------

    %--------------------------------------------------------------------------
    % 4) ファイルクローズ
    fclose(fidIn);
    fclose(fidOut);
    fprintf('Converted "%s" → "%s"\n', oldAscFile, newAscFile);
    %--------------------------------------------------------------------------
end

% runConvertOld2NewAsc
% Define file names
oldAscFile = 'old.asc';
excelFile  = 'vars.xlsx';
newAscFile = 'new.asc';

% Check that files exist
if ~isfile(oldAscFile)
    error('Old ASC file "%s" not found.', oldAscFile);
end
if ~isfile(excelFile)
    error('Excel file "%s" not found.', excelFile);
end

% Perform conversion
convertOld2NewAsc(oldAscFile, excelFile, newAscFile);

```


