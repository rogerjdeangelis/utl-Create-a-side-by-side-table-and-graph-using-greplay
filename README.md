# utl-Create-a-side-by-side-table-and-graph-using-greplay
Create a side by side table and graph using sas greplay
    %let pgm=utl-Create-a-side-by-side-table-and-graph-using-greplay;
    %let pgm=utl_table_graph_xls;

    Create a side by side table and graph using sas greplay

    github
    https://tinyurl.com/2p9y4wth
    https://github.com/rogerjdeangelis/utl-Create-a-side-by-side-table-and-graph-using-greplay

    output png file
    https://tinyurl.com/2wy6pn3w
    https://github.com/rogerjdeangelis/utl-Create-a-side-by-side-table-and-graph-using-greplay/blob/main/utlclass.png

    If you want to search my repositories, just click on repositories and put this in the search box

      greplay in:readme

    There are also R grid plots

    You should get these greplay repos
    https://tinyurl.com/462zdydb
    https://github.com/rogerjdeangelis?tab=repositories&q=greplay+in%3Areadme&type=&language=&sort=


    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    Located in work directory png graph %sysfunc(pathname(work))/utlclass.png

                                                         Frequency
    Up to 40 obs from class total obs=19                 6 +          *****
    Obs    NAME       SEX    AGE    HEIGHT    WEIGHT       |          *****
                                                         4 +          *****  *****   *****
      1    Alfred      M      14     69.0      112.5       |          *****  *****   *****
      2    Alice       F      13     56.5       84.0     2 +  *****   *****  *****   *****
      3    Barbara     F      13     65.3       98.0       |  *****   *****  *****   *****  *****
      ....                                                 --------------------------------------

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    sashelp.class


    Up to 40 obs from SASHELP.CLASS total obs=19 18SEP2022:12:12:16

    Obs    NAME       SEX    AGE    HEIGHT    WEIGHT

      1    Alfred      M      14     69.0      112.5
      2    Alice       F      13     56.5       84.0
      3    Barbara     F      13     65.3       98.0
      4    Carol       F      14     62.8      102.5
      5    Henry       M      14     63.5      102.5
      6    James       M      12     57.3       83.0
      7    Jane        F      12     59.8       84.5
      8    Janet       F      15     62.5      112.5
      9    Jeffrey     M      13     62.5       84.0
     10    John        M      12     59.0       99.5
     11    Joyce       F      11     51.3       50.5
     12    Judy        F      14     64.3       90.0
     13    Louise      F      12     56.3       77.0
     14    Mary        F      15     66.5      112.0
     15    Philip      M      16     72.0      150.0
     16    Robert      M      12     64.8      128.0
     17    Ronald      M      15     67.0      133.0
     18    Thomas      M      11     57.5       85.0
     19    William     M      15     66.5      112.0

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */
      proc datasets lib=work mt=cat nodetails nolist;
       delete mycat;
      run;quit;

      proc datasets lib=work mt=data kill nodetails nolist;
      run;quit;

      options ps=24 ls=64 nocenter ;
      title;
      footnote;

      %utlfkil("%sysfunc(pathname(work))/utlclass.png");
      %utlfkil("%sysfunc(pathname(work))/utlclass.txt");

      proc catalog cat=mycat entrytype=grseg kill;
      run;quit ;

      * create replay template;
      proc greplay tc=mycat nofs;;
      tdef T1x2 des="side by side"
         1/ ulx=0  uly=100  urx=50 ury=100
            llx=0  lly=0    lrx=50 lry=0
         2/ ulx=50 uly=100  urx=100 ury=100
            llx=50 lly=0    lrx=100 lry=0 ;
      run;quit; * need run quit;

      filename grfout "%sysfunc(pathname(work))/utlclass.png";
      filename dow    "%sysfunc(pathname(work))/utlclass.txt";

      proc printto print=dow new;
      run;

      * double space;
      proc report data=sashelp.class(obs=13 keep=name age weight) headline headskip;
      title1 "  ";  * hiddon dragon;
      title2 "  ";  * hiddon dragon;
      define name / order;
      compute before name;
         line "  ";
      endcomp;
      run;quit;

      proc printto;
      run;

      goptions reset=all border vsize=4in hsize=5in
          ftext="MingLiU" htext=2 device=png gsfname=grfout display;

      title1 "  ";  * hiddon dragon;
      title2 "DATASET: %upcase(class)";
      title3 "  ";  * hiddon dragon;

      proc gprint fileref=dow gout=mycat name="pclass";
      run;quit;

      proc gchart data=sashelp.class gout=mycat;
        title1 "  ";  * hiddon dragon;
        format age 2. weight 3.;
        title2 height= 2 "DATASET: %upcase(class) HISTOGRAM AGE";
        vbar age / width=6 space=4 sumvar=weight mean name="cclass";
      run;quit;

      *filename dow clear;

      goptions display;
      proc greplay nofs igout=mycat gout=mycat tc=mycat;
        template T1x2;
        treplay 1:pclass
                2:cclass
      ;
      run;quit;

     filename grfout clear;
     filename dow    clear;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
