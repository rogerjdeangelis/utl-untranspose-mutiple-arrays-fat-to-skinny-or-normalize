# utl-untranspose-mutiple-arrays-fat-to-skinny-or-normalize
Untranspose mutiple arrays fat yo skinny or normalize
    %let pgm=utl-untranspose-mutiple-arrays-fat-to-skinny-or-normalize;

    Untranspose mutiple arrays fat yo skinny or normalize

    github
    https://tinyurl.com/3a25scd2
    https://github.com/rogerjdeangelis/utl-untranspose-mutiple-arrays-fat-to-skinny-or-normalize

    StackOverflow R
    https://tinyurl.com/4nwvuyjs
    https://stackoverflow.com/questions/76214813/reshaping-a-dataframe-from-wide-to-long-format-in-r-using-multiple-sets-of-varia

        SOLUTIONS

            1. SAS untranspose macro (one liner)
               %untranspose(data=have,out=want,by=id country sex,id=wave, var=iv yr age);

            2. WPS untranspose (one liner)
               /*---- no changes needed to macro ----*/
               %untranspose(data=have,out=want,by=id country sex,id=wave, var=iv yr age);

            3. WPS Proc R pivot ( a little bit of Klingon - native R code)

            4. SAS/WPS SQL

            5. WPS Pro R  SQL

            6. Python SQL

            6. WPS Python SQL

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data have;
     input id country $ sex iv1 iv2 iv3 yr1 yr2 yr3 age1 age2 age3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* WORK.HAVE total obs=3 10MAY2023:10:08:21                                                                               */
    /*                                                                                                                        */
    /*  ID    COUNTRY    SEX    IV1    IV2    IV3     YR1     YR2     YR3    AGE1    AGE2    AGE3                             */
    /*                                                                                                                        */
    /*   1    UK          1      1      2      1     2007    2010    2012     60      63      65                              */
    /*   2    Spain       1      2      2      1     2008    2009    2012     40      41      44                              */
    /*   3    Sweden      2      2      2      1     2007    2010    2013     50      53      56                              */
    /*                                                                                                                        */
    /*  RULE (STACK Iv, YR and AGE ARRAYS by ID SEX                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  WORK.WANT total obs=9 10MAY2023:10:09:31                                                                              */
    /*                                                                                                                        */
    /*     ID    COUNTRY    SEX    WAVE    IV     YR     AGE                                                                  */
    /*                                                                                                                        */
    /*      1    UK          1       1      1    2007     60                                                                  */
    /*      1    UK          1       2      2    2010     63                                                                  */
    /*      1    UK          1       3      1    2012     65                                                                  */
    /*      2    Spain       1       1      2    2008     40                                                                  */
    /*      2    Spain       1       2      2    2009     41                                                                  */
    /*      2    Spain       1       3      1    2012     44                                                                  */
    /*      3    Sweden      2       1      2    2007     50                                                                  */
    /*      3    Sweden      2       2      2    2010     53                                                                  */
    /*      3    Sweden      2       3      1    2013     56                                                                  */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*                           _
     ___  __ _ ___   _   _ _ __ | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
    / __|/ _` / __| | | | | `_ \| __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
    \__ \ (_| \__ \ | |_| | | | | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
    |___/\__,_|___/  \__,_|_| |_|\__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
                                                        |_|
    */
    %untranspose(data=have,out=want,by=id country sex,id=wave, var=iv yr age);
    /*                                _
    __      ___ __  ___   _   _ _ __ | |_ _ __ __ _ _ __  ___ _ __   ___  ___  ___
    \ \ /\ / / `_ \/ __| | | | | `_ \| __| `__/ _` | `_ \/ __| `_ \ / _ \/ __|/ _ \
     \ V  V /| |_) \__ \ | |_| | | | | |_| | | (_| | | | \__ \ |_) | (_) \__ \  __/
      \_/\_/ | .__/|___/  \__,_|_| |_|\__|_|  \__,_|_| |_|___/ .__/ \___/|___/\___|
             |_|                                             |_|
    */
    libname sd1 "d:/sd1";
    data sd1.have;
     input id country $ sex iv1 iv2 iv3 yr1 yr2 yr3 age1 age2 age3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;

    %utl_wpsbegin;
    parmcards4;
    proc datasets lib=work nodetails nolist;
      delete want;
    run;quit;
    libname wrk "d:/sd1";
    %untranspose(data=wrk.have,out=want,by=id country sex,id=wave, var=iv yr age);
    proc print data=want;
    run;quit;
    ;;;;
    %utl_wpsend;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* Obs    ID    COUNTRY    SEX        WAVE    IV     YR     AGE                                                           */
    /*                                                                                                                        */
    /*  1      1    UK          1            1     1    2007     60                                                           */
    /*  2      1    UK          1            2     2    2010     63                                                           */
    /*  3      1    UK          1            3     1    2012     65                                                           */
    /*  4      2    Spain       1            1     2    2008     40                                                           */
    /*  5      2    Spain       1            2     2    2009     41                                                           */
    /*  6      2    Spain       1            3     1    2012     44                                                           */
    /*  7      3    Sweden      2            1     2    2007     50                                                           */
    /*  8      3    Sweden      2            2     2    2010     53                                                           */
    /*  9      3    Sweden      2            3     1    2013     56                                                           */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*                    ____          _            _
    __      ___ __  ___  |  _ \   _ __ (_)_   _____ | |_
    \ \ /\ / / `_ \/ __| | |_) | | `_ \| \ \ / / _ \| __|
     \ V  V /| |_) \__ \ |  _ <  | |_) | |\ V / (_) | |_
      \_/\_/ | .__/|___/ |_| \_\ | .__/|_| \_/ \___/ \__|
             |_|                 |_|
    */
    data have;
     input id country $ sex iv_w1 iv_w2 iv_w3 yr_w1 yr_w2 yr_w3 age_w1 age_w2 age_w3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;

    %let _pth=%sysfunc(pathname(work));

    %utl_submit_wps64("
    libname wrk '&_pth';
    proc r;
    export data=wrk.have r=have;
    submit;
    library(tidyr);
    library(dplyr);
    have;
    want_r<-pivot_longer(have
            ,cols=-c(ID,COUNTRY,SEX)
            ,names_to=c('.value', 'wave')
            ,names_pattern='(.*)_(W.)') %>%
        arrange(wave);
    endsubmit;
    import data=wrk.want_r r=want_r;
    run;quit;
    ");

    proc print data=want_r;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /*   ID COUNTRY SEX IV_W1 IV_W2 IV_W3 YR_W1 YR_W2 YR_W3 AGE_W1 AGE_W2 AGE_W3                                              */
    /* 1  1      UK   1     1     2     1  2007  2010  2012     60     63     65                                              */
    /* 2  2   Spain   1     2     2     1  2008  2009  2012     40     41     44                                              */
    /* 3  3  Sweden   2     2     2     1  2007  2010  2013     50     53     56                                              */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                                 _
     ___  __ _ ___     __      ___ __  ___   ___  __ _| |
    / __|/ _` / __|____\ \ /\ / / `_ \/ __| / __|/ _` | |
    \__ \ (_| \__ \_____\ V  V /| |_) \__ \ \__ \ (_| | |
    |___/\__,_|___/      \_/\_/ | .__/|___/ |___/\__, |_|
                                |_|                 |_|
    */

    data have;
     input id country $ sex iv1 iv2 iv3 yr1 yr2 yr3 age1 age2 age3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;

    /*---- this may no be as slow as you thing - depends on the compiler ----*/

    %array(_tre,values=1-3);

    proc sql;
      %do_over(_tre,phrase=%str(
           select id, country, sex, IV? as iv, YR? as yr, AGE? as age from have
           ),between=union
       );
    ;quit;

    /*---- if you want the generated code ----*/

    data _null_;
      %do_over(_tre,phrase=%str(
           put "union select id, country, sex, IV? as iv, YR? as yr, AGE? as age from have";)
       )
    run;quit;

    /*---- union select id, country, sex, IV1 as iv, YR1 as yr, AGE1 as age from have ----*/
    /*---- union select id, country, sex, IV2 as iv, YR2 as yr, AGE2 as age from have ----*/
    /*---- union select id, country, sex, IV3 as iv, YR3 as yr, AGE3 as age from have ----*/

    /*                _
     _ __   ___  __ _| |
    | `__| / __|/ _` | |
    | |    \__ \ (_| | |
    |_|    |___/\__, |_|
                   |_|
    */

    /*---- nice that sql is not case sensitive - same name but different content is crazy variable VARIABLE ----*/

    %let _pth=%sysfunc(pathname(work));
    %utl_submit_wps64("
    libname wrk '&_pth';
    proc r;
    export data=wrk.have r=have;
    submit;
    library(sqldf);
    want_rsql <- sqldf('
               select id, country, sex, iv1 as iv, yr1 as yr, age1 as age from have
         union select id, country, sex, iv2 as iv, yr2 as yr, age2 as age from have
         union select id, country, sex, iv3 as iv, yr3 as yr, age3 as age from have
      ');
    want_rsql;
    endsubmit                   ;
    ");

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /*   ID COUNTRY SEX iv   yr age                                                                                           */
    /* 1  1      UK   1  1 2007  60                                                                                           */
    /* 2  1      UK   1  1 2012  65                                                                                           */
    /* 3  1      UK   1  2 2010  63                                                                                           */
    /* 4  2   Spain   1  1 2012  44                                                                                           */
    /* 5  2   Spain   1  2 2008  40                                                                                           */
    /* 6  2   Spain   1  2 2009  41                                                                                           */
    /* 7  3  Sweden   2  1 2013  56                                                                                           */
    /* 8  3  Sweden   2  2 2007  50                                                                                           */
    /* 9  3  Sweden   2  2 2010  53                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*           _   _                             _
     _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
    | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
    |_|    |___/                                |_|
    */
    libname sd1 "d:/sd1";
    data sd1.have;
     input id country $ sex iv1 iv2 iv3 yr1 yr2 yr3 age1 age2 age3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;

    proc datasets lib=work kill nodetails nolist;
    run;quit;

    %utlfkil(d:/xpt/res.xpt);

    %utl_pybegin;
    parmcards4;
    from os import path
    import pandas as pd
    import xport
    import xport.v56
    import pyreadstat
    import numpy as np
    import pandas as pd
    from pandasql import sqldf
    mysql = lambda q: sqldf(q, globals())
    from pandasql import PandaSQL
    pdsql = PandaSQL(persist=True)
    sqlite3conn = next(pdsql.conn.gen).connection.connection
    sqlite3conn.enable_load_extension(True)
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll')
    mysql = lambda q: sqldf(q, globals())
    have, meta = pyreadstat.read_sas7bdat("d:/sd1/have.sas7bdat")
    print(have);
    res = pdsql("""
               select id, country, sex, iv1 as iv, yr1 as yr, age1 as age from have
         union select id, country, sex, iv2 as iv, yr2 as yr, age2 as age from have
         union select id, country, sex, iv3 as iv, yr3 as yr, age3 as age from have
    """)
    print(res);
    ds = xport.Dataset(res, name='res')
    with open('d:/xpt/res.xpt', 'wb') as f:
        xport.v56.dump(ds, f)
    ;;;;
    %utl_pyend;

    libname pyxpt xport "d:/xpt/res.xpt";

    proc contents data=pyxpt._all_;
    run;quit;

    proc print data=pyxpt.res;
    run;quit;

    data res;
       set pyxpt.res;
    run;quit;
    /*                                _   _                             _
    __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
    \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
      \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
             |_|         |_|    |___/                                |_|
    */


    libname sd1 "d:/sd1";
    data sd1.have;
     input id country $ sex iv1 iv2 iv3 yr1 yr2 yr3 age1 age2 age3;
    cards4;
     1 UK        1    1    2    1 2007 2010 2012   60   63   65
     2 Spain     1    2    2    1 2008 2009 2012   40   41   44
     3 Sweden    2    2    2    1 2007 2010 2013   50   53   56
    ;;;;
    run;quit;

    proc datasets lib=work kill nodetails nolist;
    run;quit;

    %utlfkil(d:/xpt/res.xpt);

    %utl_wpsbegin;
    parmcards4;
    libname sd1 "d:/sd1";
    proc python;
    export data=sd1.have python=have;
    submit;
    from os import path
    import pandas as pd
    import numpy as np
    from pandasql import sqldf
    mysql = lambda q: sqldf(q, globals())
    from pandasql import PandaSQL
    pdsql = PandaSQL(persist=True)
    sqlite3conn = next(pdsql.conn.gen).connection.connection
    sqlite3conn.enable_load_extension(True)
    sqlite3conn.load_extension('c:/temp/libsqlitefunctions.dll')
    mysql = lambda q: sqldf(q, globals())
    want_py = pdsql("""
               select id, country, sex, iv1 as iv, yr1 as yr, age1 as age from have
         union select id, country, sex, iv2 as iv, yr2 as yr, age2 as age from have
         union select id, country, sex, iv3 as iv, yr3 as yr, age3 as age from have
    """)
    print(want_py);
    endsubmit;
    ;;;;
    %utl_wpsend;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* The PYTHON Procedure                                                                                                   */
    /*                                                                                                                        */
    /*     ID   COUNTRY  SEX   iv      yr   age                                                                               */
    /* 0  1.0  UK        1.0  1.0  2007.0  60.0                                                                               */
    /* 1  1.0  UK        1.0  1.0  2012.0  65.0                                                                               */
    /* 2  1.0  UK        1.0  2.0  2010.0  63.0                                                                               */
    /* 3  2.0  Spain     1.0  1.0  2012.0  44.0                                                                               */
    /* 4  2.0  Spain     1.0  2.0  2008.0  40.0                                                                               */
    /* 5  2.0  Spain     1.0  2.0  2009.0  41.0                                                                               */
    /* 6  3.0  Sweden    2.0  1.0  2013.0  56.0                                                                               */
    /* 7  3.0  Sweden    2.0  2.0  2007.0  50.0                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
