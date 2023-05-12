# q2c 0.2.0
{q2c}: Qualtrics to Conjoint

※ **注意:** 1-2年前の書きかけのものがあったので、少しだけ修正し、とりあえずアップロードしたものです。今後、（時間があれば）普通に使えるようにします。今でも使う分には問題ないと思いますが...

## インストール

```r
remotes::install_github("JaehyunSong/q2c")
# または
devtools::install_github("JaehyunSong/q2c")
# または
pacman::p_install_gh("JaehyunSong/q2c")
```

## 使い方

* (臨時) サンプルデータのURL
   * <https://www.jaysong.net/software/Data/q2c_sample.csv>

```r
q2c(data, prefix = "F", id, outcome, covariates, type = "choice")
```

**引数**

* `data`: 読み込み済みのデータフレーム、またはデータ（`.csv`形式）のパス
* `prefix`: 属性と水準列名の接頭詞（既定値は"F"）
* `id`: 回答者のIDが格納されている列名
* `outcome`: 応答変数の列名で構成されたCharacter型ベクトル
* `covariates`: 共変量の列名で構成されたCharacter型ベクトル
* `type`: 応答変数のタイプ (`"choice"` (既定値), `"rating"`, or `"rank"`)

## 例

### 応答変数が「選択」の場合

* 応答変数のベクトルにおける変数名の順番はタスク番号と一致する必要がある。

**Input:**

```r
q2c::q2c("q2c_sample.csv", 
         prefix = "F", id = ResponseId,
         outcome = c(C1_1, C2_1, C3_1))
```

**Output:**

```
# A tibble: 600 × 15                                                 
   ID          Task Profile Outcome `소속 정당` 성별  연령  `출신 지역`
    0s<chr>      <dbl>   <dbl>   <dbl> <chr>       <chr> <chr> <chr>      
 1 R_026v5ru…     1       1       0 더불어 민…  여성  25세  영남 (경상)
 2 R_026v5ru…     1       2       1 더불어 민…  남성  45세  충청권     
 3 R_026v5ru…     2       1       0 더불어 민…  남성  55세  수도권     
 4 R_026v5ru…     2       2       1 정의당      남성  45세  영남 (경상)
 5 R_026v5ru…     3       1       1 더불어 민…  여성  25세  충청권     
 6 R_026v5ru…     3       2       0 정의당      남성  65세  수도권     
 7 R_06gxsBS…     1       1       0 정의당      여성  35세  호남 (전라)
 8 R_06gxsBS…     1       2       1 정의당      남성  45세  호남 (전라)
 9 R_06gxsBS…     2       1       0 정의당      여성  25세  충청권     
10 R_06gxsBS…     2       2       1 정의당      남성  35세  영남 (경상)
# … with 590 more rows, and 7 more variables: 전직 <chr>,
#   `집회 및 시위의 자유 보장` <chr>, `한미동맹 강화` <chr>,
#   `코로나 대책` <chr>, `세금과 복지` <chr>, `인도적 대북지원` <chr>,
#   탈원전 <chr>
```

### 応答変数が「評価」の場合

* 応答変数のベクトルにおける変数名の順番はタスク番号と一致する必要がある。

**Input:**

```r
q2c::q2c("q2c_sample.csv", 
         prefix = "F", id = ResponseId,
         outcome = c(C1_2_1, C1_2_2, C2_2_1, C2_2_2, C3_2_1, C3_2_2),
         covariates = F1:F6,
         type = "rating")
```

**Output:**

```
# A tibble: 600 × 21                                                 
   ID          Task Profile Outcome `소속 정당` 성별  연령  `출신 지역`
    0s<chr>      <dbl>   <dbl>   <dbl> <chr>       <chr> <chr> <chr>      
 1 R_026v5ru…     1       1       4 더불어 민…  여성  25세  영남 (경상)
 2 R_026v5ru…     1       2       4 더불어 민…  남성  45세  충청권     
 3 R_026v5ru…     2       1       3 더불어 민…  남성  55세  수도권     
 4 R_026v5ru…     2       2       6 정의당      남성  45세  영남 (경상)
 5 R_026v5ru…     3       1       5 더불어 민…  여성  25세  충청권     
 6 R_026v5ru…     3       2       5 정의당      남성  65세  수도권     
 7 R_06gxsBS…     1       1       8 정의당      여성  35세  호남 (전라)
 8 R_06gxsBS…     1       2       7 정의당      남성  45세  호남 (전라)
 9 R_06gxsBS…     2       1       5 정의당      여성  25세  충청권     
10 R_06gxsBS…     2       2       5 정의당      남성  35세  영남 (경상)
# … with 590 more rows, and 13 more variables: 전직 <chr>,
#   `집회 및 시위의 자유 보장` <chr>, `한미동맹 강화` <chr>,
#   `코로나 대책` <chr>, `세금과 복지` <chr>, `인도적 대북지원` <chr>,
#   탈원전 <chr>, F1 <dbl>, F2 <dbl>, F4 <dbl>, F3 <dbl>, F5 <dbl>,
#   F6 <dbl>
```

### 応答変数が「順位」の場合

* 「評価」と同じ。`type = "rank"`を指定（実は`"rating"`のままでも良い）
