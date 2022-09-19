  proc datasets lib=work mt=cat nodetails nolist;                                                                                       
   delete mycat;                                                                                                                        
  run;quit;                                                                                                                             
                                                                                                                                        
  proc datasets lib=work mt=data kill nodetails nolist;                                                                                 
  run;quit;                                                                                                                             
                                                                                                                                        
  options ps=24 ls=64 nocenter ;                                                                                                        
  title;                                                                                                                                
  footnote;                                                                                                                             
                                                                                                                                        
  %utlfkil("%sysfunc(pathname(work))/utlclass.pdf");                                                                                    
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
                                                                                                                                        
  filename grfout "%sysfunc(pathname(work))/utlclass.pdf";                                                                              
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
                                                                                                                                        
  goptions reset=all border vsize=5in hsize=8in                                                                                         
      ftext="MingLiU" htext=1 device=pdfc gsfname=grfout display;                                                                       
                                                                                                                                        
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
