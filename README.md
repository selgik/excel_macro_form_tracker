# Automate data entry process with macro-enabled spreadsheet
### Table of Contents
    1. Why should I automate a data entry process? 
    2. What do I want to automate? 
    3. How does the schema look like?
    4. How does the final deliverables look like? 

### 1. Why should I automate a data entry process? 
    Task:
    I am working in a QA project as a reviewer. I am checking contents* that has an unique reference ID**.
    If I notice an error in the content, I track its reference ID, categorize errors and leave comments in the spreadsheet.
    Once target amounts of errors are captured, I clean the spreadsheet and repeat the process next day.

    Problem:
    a. I need to choose 3 types of hirarchiar errors from the list manually.
    b. I need to copy and paste the reference ID somewhere else and just track the certain part of it.
    c. I need to manually count the various types of error and I easily lose a track of it.
    d. I need to copy and paste the table template every day. Or, I drag the table and clear contents manually.
   
    Motivation:
    I want to automate manual and repetitive process to reduce time and errors.
    I anticipate the macro-enabled spreadsheet will at least reduce 50% of steps invovled in the tracking process.
     
    * Contents refer to any type of texst, photo, audio or video files. 
    ** An example of reference ID can be "https://sample.address.com/sample1/error123/phototype121" or "14##AB34aood". 

### 2. What do I want to automate?
    (1) I notice an error in the content. 
        I need to track reference ID, error types and comments for the content ("error data"). 
        --> (2) I enter error data into macro-enabled data entry form ("entry_form"). 
                Currently error data sits under the spreadsheet1 ("entry_form_worksheet") containing entry_form. 
                I click insert button built in the entry_form. 
                --> (3) Clicking insert button will trigger error data to be transffered from entry_form_worksheet to the linked worksheet2 ("tracker_worksheet"). 
                        Tracker_worksheet contains tracking table ("tracker_table").
                        By transffering, macro will feed error data to targeted cells in the tracker_table.
                        Once transffered, macro will erase the entered data from the entry_form.
                        --> (4) Upon the transfer, error types will be counted in the table ("count_table") next to the entry_form in entry_form_worksheet.
                                counter_table use COUNTIF range from tracker_table. 
                                Once the erorr types hit targeted number, error types cell and/or row will be highlighted. 
                                Reviewer can stop tracking highlighted error type. 
                                --> (5) Next set of error data will trigger adding next record (to the next line of the row) in the tracker_table. 
                                        Process (1)-(4) will be repeated. 
                 
### 3. How does the schema look like?
    (1) Worksheet:
        WORKSHEET NAME            DESCRIPTION 
        a. entry_form_worksheet   Source worksheet where entry_form and count_table resides.    
        b. tracker_worksheet      Target worksheet where tracker table resides.
      
    (2-1) Table: entry_form_worksheet.entry_form
        FIELD/RECORD NAME         TYPE            MODE            DESCRIPTION
        a. Reference ID#          STRING          Required        Web link, file address or content reference number that needs to be tracked.
        b. Error                  STRING          Required        Error type that is selectable from the list (data validation inserted).
        c. Comment                STRING          Nullable        Comments section with/without characters limitation.
            
    (2-3) Table: tracker_worksheet.tracker_table
        FIELD NAME                TYPE            MODE            DESCRIPTION
        a. Date                   Date            Required        Display today's date via =TODAY()
        b. Name                   STRING          Required        Reviewer's name. 
        c. Core ID#               STRING          Required        Substring of 2.(2-1)a, manipulated via LEFT,RIGHT or MID.
        d. Error                  STRING          Required        2.(2-1)b
        e. Error group            STRING          Required        Hirarchial value of 2.(2-1)b, pulled out via VLOOKUP 
        f. Error type             STRING          Required        Hirarchial value of 2.(2-1)b, pulled out via VLOOKUP  
        g. Comment                STRING          Nullable        2.(2-1)c
     
    (2-4) Table: entry_form_worksheet.count_table
        FIELD/RECORD NAME         TYPE            MODE            DESCRIPTION
        a. Error                 STRING           Required        2.(2-1)b. Optionally, Error group/type can come together.
        b. Count                 INTEGER          Required        Starts from 0. COUNTIF Formula used, conditional fomatting applied.

   
### 4. How does the final deliverable look like?
       Depending on the scenario, two types of macro-enabled excel spreadsheet can be created.
      
       (1) Type: If the content you are reviewing would generally contain 1-3 types of errors, track errors per error type. 
                 Scenario  : You need to track two types of error, type A and B for content 14##AB34aood.
                 Use guide : You track error type A and use Insert_Button(). Repeat the process to track the error type B. 
                 Results   : You will have two records generated in 2.(1)b, for error type A and B.
       (2) Type: If the content you are reviewing would generally contain 3+ types of errors, track errors per content. 
                 Scenario  : You need to track 4 types of error, type A, B, C and D.
                 Use guide : You track error type A, B, C, D and use Insert_Button().
                 Results   : You will have four records generated in 2.(1)b, for type A, B, C and D.

-- END -- 
<!--
- Summary: Macro-enabled data entry form to simplify tracking process.
- Purpose: The goal of this project is to minimize the repetitive process of data entry and human errors associated with it. This project will streamline extraction of string values and pulling out hierarchical values using data entry form in macro-enabled Excel spreadsheet. The final deliverable will reduce at least 50% of the steps involved in the data entry process.
--->
