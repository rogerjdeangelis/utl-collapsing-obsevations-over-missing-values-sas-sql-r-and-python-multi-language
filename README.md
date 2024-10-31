# utl-collapsing-obsevations-over-missing-values-sas-sql-r-and-python-multi-language
Collapsing observations over missing values sas sql r and python multi language
    %let pgm=utl-collapsing-obsevations-over-missing-values-sas-sql-r-and-python-multi-language;

    Collapsing obsevations over missing values sas sql r and python multi language

    github
    https://tinyurl.com/mv2426zu
    https://github.com/rogerjdeangelis/utl-collapsing-obsevations-over-missing-values-sas-sql-r-and-python-multi-language

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                   |                                    |                                               */
    /*               INPUT               |           PROCESS                  |          OUTPUT                               */
    /*                                   |                                    |                                               */
    /*  STATE    YEAR    R1    R2    R3  |  COLLAPSE REMOVING MISSINGS        | STATE    YEAR    R1    R2    R3               */
    /*                                   |                                    |                                               */
    /*   AL      2010    15     .     .  |                                    |  AL      2010    15    18    20               */
    /*   AL      2010     .    18     .  |                                    |  AL      2011    18    17    22               */
    /*   AL      2010     .     .    20  |  AL      2010    15    18    20    |  MA      2010    10    11    12               */
    /*                                   |                                    |  MA      2011    11    13    15               */
    /*   AL      2011    18     .     .  |                                    |                                               */
    /*   AL      2011     .    17     .  |                                    |                                               */
    /*   AL      2011     .     .    22  |  AL      2011    18    17    22    |                                               */
    /*                                   |                                    |                                               */
    /*   MA      2010    10     .     .  |                                    |                                               */
    /*   MA      2010     .    11     .  |                                    |                                               */
    /*   MA      2010     .     .    12  |  MA      2010    10    11    12    |                                               */
    /*                                   |                                    |                                               */
    /*   MA      2011    11     .     .  |                                    |                                               */
    /*   MA      2011     .    13     .  |                                    |                                               */
    /*   MA      2011     .     .    15  |  MA      2011    11    13    15    |                                               */
    /*                                   |                                    |                                               */
    /*                                   |                                    |                                               */
    /*                                   |  SAME SOLUTION SAS, R, AND PYTHON  |                                               */
    /*                                   |  ================================  |                                               */
    /*                                   |                                    |                                               */
    /*                                   |  select                            |                                               */
    /*                                   |    state                           |                                               */
    /*                                   |   ,year                            |                                               */
    /*                                   |   ,max(r1) as r1                   |                                               */
    /*                                   |   ,max(r2) as r2                   |                                               */
    /*                                   |   ,max(r3) as r3                   |                                               */
    /*                                   |  from                              |                                               */
    /*                                   |    sd1.have                        |                                               */
    /*                                   |   group                            |                                               */
    /*                                   |     by state, year                 |                                               */
    /*                                   |                                    |                                               */
    /**************************************************************************************************************************/

       SOLUTIONS

          0 sas update (many other solutions summary, dow loop report, means...)
          1 sas sql
          2 r sql
          3 python sql

    SAS Community
    https://stackoverflow.com/questions/79094984/combine-multiple-observations-into-one-sas

    libname sd1 "d:/sd1";
    options validvarname=upcase;
    data sd1.have;
    input State$ year r1-r3;
    cards4;
    AL 2010 15 . .
    AL 2010 . 18 .
    AL 2010 . . 20
    AL 2011 18 . .
    AL 2011 . 17 .
    AL 2011 . . 22
    MA 2010 10 . .
    MA 2010 . 11 .
    MA 2010 . . 12
    MA 2011 11 . .
    MA 2011 . 13 .
    MA 2011 . . 15
    ;;;;
    run;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  STATE    YEAR    R1    R2    R3                                                                                       */
    /*                                                                                                                        */
    /*   AL      2010    15     .     .                                                                                       */
    /*   AL      2010     .    18     .                                                                                       */
    /*   AL      2010     .     .    20                                                                                       */
    /*   AL      2011    18     .     .                                                                                       */
    /*   AL      2011     .    17     .                                                                                       */
    /*   AL      2011     .     .    22                                                                                       */
    /*   MA      2010    10     .     .                                                                                       */
    /*   MA      2010     .    11     .                                                                                       */
    /*   MA      2010     .     .    12                                                                                       */
    /*   MA      2011    11     .     .                                                                                       */
    /*   MA      2011     .    13     .                                                                                       */
    /*   MA      2011     .     .    15                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                    _       _
     / _ \   ___  __ _ ___   _   _ _ __   __| | __ _| |_ ___
    | | | | / __|/ _` / __| | | | | `_ \ / _` |/ _` | __/ _ \
    | |_| | \__ \ (_| \__ \ | |_| | |_) | (_| | (_| | ||  __/
     \___/  |___/\__,_|___/  \__,_| .__/ \__,_|\__,_|\__\___|
                                  |_|
    */

    data updatewant;
      update sd1.have(obs=0) sd1.have;
      by state year;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  STATE    YEAR    R1    R2    R3                                                                                       */
    /*                                                                                                                        */
    /*   AL      2010    15    18    20                                                                                       */
    /*   AL      2011    18    17    22                                                                                       */
    /*   MA      2010    10    11    12                                                                                       */
    /*   MA      2011    11    13    15                                                                                       */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;
      create
        table saswant as
      select
        state
       ,year
       ,max(r1) as r1
       ,max(r2) as r2
       ,max(r3) as r3
      from
        sd1.have
       group
         by state, year
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Obs    STATE    YEAR    R1    R2    R3                                                                                 */
    /*                                                                                                                        */
    /*  1      AL      2010    15    18    20                                                                                 */
    /*  2      AL      2011    18    17    22                                                                                 */
    /*  3      MA      2010    10    11    12                                                                                 */
    /*  4      MA      2011    11    13    15                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(sqldf)
    library(haven)
    source("c:/oto/fn_tosas9x.r")
    set.seed(1)
    have<-read_sas("d:/sd1/have.sas7bdat")
    have;
    want <- sqldf("
      select
        state
       ,year
       ,max(r1) as r1
       ,max(r2) as r2
       ,max(r3) as r3
      from
        have
       group
         by state, year
    ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R                       SAS                                                                                           */
    /*                                                                                                                        */
    /*   STATE YEAR r1 r2 r3    OWNAMES    STATE    YEAR    R1    R2    R3                                                    */
    /*                                                                                                                        */
    /* 1    AL 2010 15 18 20       1        AL      2010    15    18    20                                                    */
    /* 2    AL 2011 18 17 22       2        AL      2011    18    17    22                                                    */
    /* 3    MA 2010 10 11 12       3        MA      2010    10    11    12                                                    */
    /* 4    MA 2011 11 13 15       4        MA      2011    11    13    15                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    print(have)
    want=pdsql("""
      select
        state
       ,year
       ,max(r1) as r1
       ,max(r2) as r2
       ,max(r3) as r3
      from
        have
       group
         by state, year
       """);
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* Python                              SAS                                                                                */
    /*                                                                                                                        */
    /*   STATE  YEAR    r1    r2    r3     STATE    YEAR    R1    R2    R3                                                    */
    /*                                                                                                                        */
    /* 0   AL  2010.0  15.0  18.0  20.0     AL      2010    15    18    20                                                    */
    /* 1   AL  2011.0  18.0  17.0  22.0     AL      2011    18    17    22                                                    */
    /* 2   MA  2010.0  10.0  11.0  12.0     MA      2010    10    11    12                                                    */
    /* 3   MA  2011.0  11.0  13.0  15.0     MA      2011    11    13    15                                                    */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
