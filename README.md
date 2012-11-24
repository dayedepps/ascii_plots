# ascii_plots
These are just some silly scripts which I like to have on my command line when I'm doing quick and dirty 
data analysis and can't be bothered to start R. They all receive the data by piping, typically downstream of awk, cut... 

They all handle non-numeric data as NA.

# author
[Daniel Zerbino](https://github.com/dzerbino/)


# Use
For a quick demo, run:

    > sh demo.sh

## cor - correlation:
Takes in stdin a file with two columns, print out Pearson correlation.

    > cut -f1,2 test.tsv | ./cor
    0.987425

## summary:
Takes in stdin a tab delimited data file with or without headers (anything numeric is assumed to be data, anything else NA) and prints out basic stats on each column (position, header (or first value), min, mean, max, sum)

    > cat test.tsv | ./summary
    COL |           1 |          2 |          3 |          4 |
    NAM |           A |          B |          C |          D |
    MIN |           7 |          5 |          9 |          0 |
    AVG |           3 |       1.75 |       3.75 |          0 |
    MAX |           7 |          5 |          9 |          0 |
    SUM |          12 |          7 |         15 |          0 |

## hist - histogram:
Either:
* Takes in a single column of numbers, displays histogram
* Takes in a double column of numbers, and displays a weighted histogram of the data, assuming the first column are values and the second column weights. The size of the bins is 1 by default, but can be specified as an option

```shell
    > awk 'func r(){return sqrt(-2*log(rand()))*cos(6.2831853*rand())}BEGIN{for(i=0;i<10000;i++)s=s"\n"0.5*r();print s}' | ./hist 0.1
    -1.800000 |      1 | 
    -1.600000 |      4 | 
    -1.500000 |      5 | 
    -1.400000 |      7 | 
    -1.300000 |     18 | *
    -1.200000 |     40 | **
    -1.100000 |     58 | ****
    -1.000000 |     85 | ******
    -0.900000 |    126 | *********
    -0.800000 |    197 | **************
    -0.700000 |    285 | *********************
    -0.600000 |    349 | **************************
    -0.500000 |    422 | *******************************
    -0.400000 |    532 | ***************************************
    -0.300000 |    634 | ***********************************************
    -0.200000 |    681 | **************************************************
    -0.100000 |    756 | ********************************************************
     0.000000 |   1557 | ********************************************************************************************************************
     0.100000 |    743 | *******************************************************
     0.200000 |    698 | ****************************************************
     0.300000 |    628 | **********************************************
     0.400000 |    546 | ****************************************
     0.500000 |    420 | *******************************
     0.600000 |    351 | **************************
     0.700000 |    252 | ******************
     0.800000 |    208 | ***************
     0.900000 |    140 | **********
     1.000000 |    104 | *******
     1.100000 |     65 | ****
     1.200000 |     35 | **
     1.300000 |     20 | *
     1.400000 |     14 | *
     1.500000 |      9 | 
     1.600000 |      5 | 
     1.700000 |      1 | 
     1.800000 |      1 | 
     1.900000 |      2 | 
     2.000000 |      1 | 
    TOTAL     |  10000 |
```
## scatter:
Takes in a double column of numbers, and displays a sketchy ascii density plot.

    > awk 'func r(){return sqrt(-2*log(rand()))*cos(6.2831853*rand())}BEGIN{for(i=0;i<10000;i++)s=s"\n"0.5*r()"\t"0.5*r();print s}' | ./scatter
    ---------------------------------------------------------------------------------------------------------------------- 2.00418
    |                                       '              '                                                             |
    |                                                    '                                                               |
    |                                               '                     '                                              |
    |                              '       '         '                                                  '                |
    |                                          '  '     '   `  ''        `'   '    '                                     |
    |                                ' '  '  '   ' '    ' `    '     '      '     '                                      |
    |                          '     '     '''  '' '   '    ' ''          '  ' '             '                           |
    |                              '     '    '' '   '   ,`' ,'`' ` '''   ` `' '  '            ''   '                    |
    |                     '      '' ` `' ' '`'' '' '  `'   ''``''`''`'';' `,  ''  ''    `' '                             |
    |                         ,     ' '''''  ````'' '`,`!' ,,;` `;`'> ``'```'``,'`  '`' `'   ' `                         |
    |                    '   '' '''`,'`; ' '``',`````;!!,,;'`;,;''! ,;,!,;'';'`, '` `''  `        '    '                 |
    |                   '    '    ,'`'`,,`,;',``,;;,`!~!;{,,!'!!!,!>!;`!~,,;'`,';`''`'' '''` ' `   '              '      |
    |                 ' `'   ` `' ',;, `' ',`!~`!; !']~{-!{~~]>;!!-!{!;';!!~;;;' !'`;`','''``'     `  '                  |
    |                  `''    '''';'',;,;'>>)>,)~;-{]|~-j~]t~)]{t~)~!->]-!>!;,!`>`,`, ,;;,'`',''                         |
    |          '   ''' ''',' `, `!``;;;,~]];!!>]!)-{]vt|vj]n-~-{|j,)>-~n!]~{~!!>'>!;`!`,`` , ` '`   `        '           |
    |               '' ' `' '`'',`;;;~!-{`~~|>{~{]v>{|)XX|v-~{otnjCtC)v;{tX)]->;)>>,`!],`;;  `;` ; '`      '      '      |
    |          '  '  '`''  , '`',`!!,!)>!-~{{j|,j|-0njC||0U0CoXn|]o-tUjC{|U)|>-]->{~{!!,;' !`  ,`'  ''  ' ' ' ' '        |
    |             '  `  `  ` !;- ;>,-;|>{~>!{]|-U]j]XUU00kjXkj00)|jtXjjttnv]n-`]{;{!~!~!-!';,''`'  '' ' ` '              |
    |   '       '   '   ' ;,``'`'`,!>!]])-|t{t|{-U{)|ntCtCnkvkvqCXZUC&Z~0||)-!|{{|;>),]!~; ,`,`';'  ''' ''      '        |
    |      ` '   '   ,`'`''``;'`', ,;{)]{{tn)]]{UvdjCZv#-jtC0tZtC|UZ0jUUUC{{0]|!>]{>~~~~{!;`,; ';;;' ' '  '             '|
    |            '  '  ' ,''``;`~,!];;>!;!|-)]CZvt{UXZqCC$ZnZ|$nokXCUUkjXt]--X|t])>`]!->``;;`>;`'' ''                '   |
    |      ''   '`''''' '', `,'>!)!!->)]|))|UtX)|tnvt{|nZvqU0nqdC0#{v)Uqnt|{--t{)])!;,>`-,,;,';`''``   ' `               |
    |         '      ,' ,'``,;'';!,>>]~~~>~]>]q)|0vnvjjCZvqvnqtX0n)qttvv{X)]t~|]j!),]' !;`,``'``'' ''''   '              |
    |           '  ' '` ````!,``'>>'{;>~;!;~{|j!)]nZtXnj|U0Udtd0njXvjj){nn>]]){{`~;>>- ,;!, `,'`,'     '       '         |
    |   '  '             '''`'` ;`>,>;,;-,~~`-)>]t~t|{-n)t{{tnU]jXUv]n-~],;-t;~;!{!~>,`;,`,`;`'` '` `''      ''          |
    |'        '    `'     '' '',,`,`;~;,,,;!-|~-tj-])v!>|]t--j)>Uv]>-~]~!;;!,~-,>'!',,'``''' ',`' ` ' '     '  '         |
    |              '  `   `'>   ',;```!;~;|!~;~->>-,]]~>-;~]))]!>!-`-)-,]-{~,;,`;`',',`'; ,`,'`''  '   '                 |
    |                     ' ' `''' ''`'',,,;;!!-,{`-];>~-,>-~~>;{!)];`;--,>`;!,;`;;`'`; ` ''''   '      `                |
    |                  '        ' ' '''` `` `';`';`,;>!,!~!~,~-;>~;!!``!,>!`',!`,`'`,, '' ,', ''''                       |
    |                '          '''  ' `'`; ''`;`;``> ';>;,!>'''>!>'`;;;;` `'' `' '`''     '           '                 |
    |         '                `  ``'  `'`  ' '''`!`;`!'`,`'` ``;;'!` `! ,'`;',` '' ' '                                  |
    |           '             ' ' ''   ' `  ,' ` `', ,'`''';'`'``''' ''''```'     `, '' `''   '                          |
    |                            '     ''   ' ''   ' `' ''' ` ' `', '''' ' ` '   '''                '                    |
    |                       ''               ' `   ' `   `'   ' '  `'      '         ' '                                 |
    |                        '        '   '      , '   ' '     '             '           '                               |
    |                                    '              ''       ' '      `                 '                            |
    |                                                         '  '              '                                        |
    |                                                                 '                                                  |
    ---------------------------------------------------------------------------------------------------------------------- -1.7106
    -1.826500                                                                                                     1.910550

## curve:
Draws a curve from a single column of numbers [NOTE: requires scatter to be in the same directory]

    > awk 'BEGIN{for(i=0;i<100;i++)s=s"\n"sin(i/10);print s}' | ./curve 
    ---------------------------------------------------------------------------------------------------------------------- 0.999574
    |               $$$$$$ $                                                                 $$$$$ $                     |
    |             $         $                                                             $ $       $$                   |
    |           $$           $                                                           $            $                  |
    |                         $                                                         $              $                 |
    |          $               $                                                                        $                |
    |         $                 $                                                      $                                 |
    |        $                                                                        $                   $              |
    |                             $                                                  $                     $             |
    |      $                       $                                                                                     |
    |     $                                                                        $                        $            |
    |                               $                                                                        $           |
    |    $                                                                        $                                      |
    |                                $                                           $                            $          |
    |   $                                                                                                                |
    |  $                              $                                         $                              $         |
    |                                  $                                                                                 |
    | $                                                                        $                                 $       |
    |                                    $                                                                               |
    |$                                                                        $                                   $      |
    |                                     $                                                                        $     |
    |                                                                        $                                           |
    |                                      $                               $                                        $    |
    |                                                                                                                    |
    |                                       $                             $                                          $   |
    |                                        $                                                                           |
    |                                                                    $                                            $  |
    |                                         $                                                                         $|
    |                                                                   $                                                |
    |                                          $                       $                                                 |
    |                                            $                                                                       |
    |                                                                 $                                                  |
    |                                             $                 $                                                    |
    |                                              $               $                                                     |
    |                                               $             $                                                      |
    |                                                $           $                                                       |
    |                                                 $         $                                                        |
    |                                                   $$$ $$ $                                                         |
    |                                                      $                                                             |
    ---------------------------------------------------------------------------------------------------------------------- -0.999923
    2.000000                                                                                                    101.000000
