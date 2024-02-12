# Lab 3 (Bugs and Commands)

## Bugs in Week 4 Lab

The bug in focus will be involving the ```ReverseInPlace()``` method located in ```.\lab3\ArrayExamples.java```. The issue the method is that it was not utilizing a temporary variable, meaning that some elements are becoming overwritten. <br>
<br>
It does not appear like buggy code in the case of an array with a single element.
```
public void testReverseInPlace() {
  int[] input1 = { 3 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 3 }, input1);
}
```
This test cases passes with no issues, which can be seen from the output of the JUnit tests here:
```
JUnit version 4.13.2
....
Time: 0.027

OK (4 tests)
```
However, once we incorporate a more comprehensive test such as providing an array with multiple elements, we can start to see bugs appearing in the program. The following test implemented will be used to test for this issue. The test is named ```testReverseInPlaceMoreElems```.
```
@Test
public void testReverseInPlaceMoreElems() {
  int[] input1 = {1, 2, 3, 4};
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{4, 3, 2, 1}, input1);
}
```
The test is inputs an array of four elements and expects the same array to be editted in reverse order. When we run the tester file now, it results in a failure in one of the JUnit tests, specifically the one that was just implemented. The following output is given.
```
JUnit version 4.13.2
...E..
Time: 0.03
There was 1 failure:
1) testReverseInPlaceMoreElems(ArrayTests)
arrays first differed at element [2]; expected:<2> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlaceMoreElems(ArrayTests.java:16)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<2> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 5,  Failures: 1
```
By utilizing a debugging print statement: ```System.err.println(Arrays.toString(input1));```, we are able to identify the symptoms. As it stands, the current output of the ```ReverseInPlace()``` method with the input of ```[1, 2, 3 ,4]``` is ```[4, 3, 3, 4]```. This allows us to identify that the method is not reversing properly and is actually overwritting some of the elements within the array.<br>
<br>
This happens because the original code does not make use of a temporary variable, meaning that while changing elements, some become overwritten as a result. The fixed code is as presented in the following block:
```
static void reverseInPlace(int[] arr) {
  for(int i = 0; i < arr.length / 2; i++) {
    int temp = arr[i];
    arr[i] = arr[arr.length - i - 1];
    arr[arr.length - i - 1] = temp;
  }
}
```
Instead of iteratoring over the entire array, we will only iterate half of it and every time, we are using a temporary variable to save the current element we are about to overwrite and swapping it with the element that is able to replace it. By implementing this change, we are able to successfully pass the tests written.

```
JUnit version 4.13.2
.....
Time: 0.021

OK (5 tests)
```

## Researching Commands
The ```find``` command is generally used to search out all files of a specific nature. By running ```man find```, we are given the following output that will help show different ways that ```find``` could be used.

```
FIND(1)                                                                               General Commands Manual                                                                               FIND(1)

NAME
       find - search for files in a directory hierarchy

SYNOPSIS
       find [-H] [-L] [-P] [-D debugopts] [-Olevel] [path...] [expression]

DESCRIPTION
       This manual page documents the GNU version of find.  GNU find searches the directory tree rooted at each given file name by evaluating the given expression from left to right, according to      
       the rules of precedence (see section OPERATORS), until the outcome is known (the left hand side is false for and operations, true for or), at which point find moves on  to  the  next  file      
       name.

       If  you are using find in an environment where security is important (for example if you are using it to search directories that are writable by other users), you should read the "Security
       Considerations" chapter of the findutils documentation, which is called Finding Files and comes with findutils.   That document also includes a lot more detail  and  discussion  than  this      
       manual page, so you may find it a more useful source of information.

OPTIONS
       The  -H,  -L  and  -P options control the treatment of symbolic links.  Command-line arguments following these are taken to be names of files or directories to be examined, up to the first      
       argument that begins with `-', or the argument `(' or `!'.  That argument and any following arguments are taken to be the expression describing what is to be searched for.  If no paths are      
       given, the current directory is used.  If no expression is given, the expression -print is used (but you should probably consider using -print0 instead, anyway).

       This  manual  page  talks  about `options' within the expression list.  These options control the behaviour of find but are specified immediately after the last path name.  The five `real'      
       options -H, -L, -P, -D and -O must appear before the first path name, if at all.  A double dash -- can also be used to signal that any remaining arguments are not options (though  ensuring
       that all start points begin with either `./' or `/' is generally safer if you use wildcards in the list of start points).
```

## ```find -type``` Command Line Option[^1]
 One way we can do this is by utilizing ```-type``` with ```find```. While we are in the working directory ```.\docsearch\```, we can run the command ```find technical/plos -type f```, it will return find all files in the directory ```.\docsearch\technical\plos``` and output all of their names.

```
darrenlov@darren:/mnt/c/Users/dlov/Desktop/cse15l/docsearch$ find technical/plos -type f
technical/plos/journal.pbio.0020001.txt
technical/plos/journal.pbio.0020010.txt
technical/plos/journal.pbio.0020012.txt
technical/plos/journal.pbio.0020013.txt
... (omitted due to length)
technical/plos/pmed.0020281.txt
```
In this case, ```-type f``` is being used to find all the files within the directory ```.\docsearch\technical\plos\```. This could be useful if a list of all the files, regardless of their type, is needed as the command excludes directories.<br>
<br>

Another way to use this command is ```find -type d```, which will only search for directories.  If we run the command ```find technical -type d``` while in the working directory ```.\docsearch\```. It will recursively search through all the possible directories within the specified directory.
```
darrenlov@darren:/mnt/c/Users/dlov/Desktop/cse15l/docsearch$ find technical -type d
technical
technical/911report
technical/biomed
technical/government
technical/government/About_LSC
technical/government/Alcohol_Problems
technical/government/Env_Prot_Agen
technical/government/Gen_Account_Office
technical/government/Media
technical/government/Post_Rate_Comm
technical/plos
```
This command is useful if you need to find all the different directories. It reminds me of the ```ls``` command but it shows how deep each directory may go. This can be seen in the output above, the directory ```technical/government``` has many other directories within it, such as ```technical/government/Alcohol_Problems```. The command would be useful to find all of those directories if needed.

## ```find -newer``` Command Line Option[^2]
The command line option ```-newer``` for find recursively searches for all files that are more recently modified than a specified reference file. If we use the command ```find -newer test.sh``` in the working directory ```.\docsearch```, it displays all files that were more recently modified than ```test.sh```.
```
./DocSearchServer.class
./FileHelpers.class
./Handler.class
./Server.class
./ServerHttpHandler.class
./TestDocSearch.class
./URLHandler.class
```
This could be useful with ```bash shell scripts``` as it may be used to check what files were modified after execution. In this case, ```test.sh``` compiles all the Java files within the working directory ```.\docsearch```, which is why the output is only files with the type of ```.class```.<br>
<br>

Another way to use ```find -newer``` is by using the command ```find -newermt```. This command allows the user to check for modification dates within a certain number of time. If the command ```find . -newermt "-1 days"``` is ran in the working directory ```.\docsearch```, the following output is given.
```
darrenlov@darren:/mnt/c/Users/dlov/Desktop/cse15l/docsearch$ find . -newermt "-1 days"
./DocSearchServer.class
./FileHelpers.class
./Handler.class
./Server.class
./ServerHttpHandler.class
```
This is could be useful because you would be able to check what files were recently modified within a specified time period. It could be used to check what files someone may have modified if another user was on the computer. The command recursively checks through all directories for any files that were modified in the past one day.

## ```find -size``` Command Line Option[^2]
The command line option ```-size``` for find recursively searches all files for files that are either larger, equal to, or smaller, depending on the specified amount. For example, if the command ```find -size -10k``` is run in the working directory ```.\docsearch```, it will search each directory within ```.\docsearch``` for any files that are smaller than ```10 kilobytes.```

```
darrenlov@darren:/mnt/c/Users/dlov/Desktop/cse15l/docsearch$ find -size -10k
.
./DocSearchServer.class
./DocSearchServer.java
./FileHelpers.class
./find-results.txt
./find-text.sh
./grep-results.txt
./Handler.class
./lib
./README.md
./Server.class
./Server.java
./ServerHttpHandler.class
./start.sh
./technical
./technical/911report
./technical/biomed
./technical/biomed/1471-2334-3-13.txt
./technical/biomed/1471-2490-3-2.txt
./technical/government
./technical/government/About_LSC
./technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
./technical/government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
./technical/government/Alcohol_Problems
./technical/government/Env_Prot_Agen
./technical/government/Gen_Account_Office
./technical/government/Gen_Account_Office/d01121g.txt
./technical/government/Gen_Account_Office/Letter_WalkerJan30-2001.txt
./technical/government/Gen_Account_Office/og96009.txt
./technical/government/Gen_Account_Office/og96015.txt
./technical/government/Gen_Account_Office/og96020.txt
./technical/government/Gen_Account_Office/og96021.txt
./technical/government/Gen_Account_Office/og96028.txt
./technical/government/Gen_Account_Office/og96032.txt
./technical/government/Gen_Account_Office/og96033.txt
./technical/government/Gen_Account_Office/og96042.txt
./technical/government/Gen_Account_Office/og97001.txt
./technical/government/Gen_Account_Office/og97002.txt
./technical/government/Gen_Account_Office/og97011.txt
./technical/government/Gen_Account_Office/og97019.txt
./technical/government/Gen_Account_Office/og97020.txt
./technical/government/Gen_Account_Office/og97023.txt
./technical/government/Gen_Account_Office/og97028.txt
./technical/government/Gen_Account_Office/og97038.txt
./technical/government/Gen_Account_Office/og97045.txt
./technical/government/Gen_Account_Office/og97046.txt
./technical/government/Gen_Account_Office/og98018.txt
./technical/government/Gen_Account_Office/og98019.txt
./technical/government/Gen_Account_Office/og98024.txt
./technical/government/Gen_Account_Office/og98026.txt
./technical/government/Gen_Account_Office/og98029.txt
./technical/government/Gen_Account_Office/og98030.txt
./technical/government/Gen_Account_Office/og98032.txt
./technical/government/Gen_Account_Office/og98040.txt
./technical/government/Gen_Account_Office/og98041.txt
./technical/government/Gen_Account_Office/og98044.txt
./technical/government/Gen_Account_Office/og98045.txt
./technical/government/Gen_Account_Office/og98046.txt
./technical/government/Gen_Account_Office/og99036.txt
./technical/government/Media
./technical/government/Media/5_Legal_Groups.txt
./technical/government/Media/Abuse_penalties.txt
./technical/government/Media/Advocate_for_Poor.txt
./technical/government/Media/agency_expands.txt
./technical/government/Media/Aid_Gets_7_Million.txt
./technical/government/Media/All_May_Have_Justice.txt
./technical/government/Media/Annual_Fee.txt
./technical/government/Media/Anthem_Payout.txt
./technical/government/Media/AP_LawSchoolDebts.txt
./technical/government/Media/Attorney_gives_his_time.txt
./technical/government/Media/Avoids_Budget_Cut.txt
./technical/government/Media/A_helping_hand.txt
./technical/government/Media/A_Perk_of_Age.txt
./technical/government/Media/balance_scales_of_justice.txt
./technical/government/Media/Barnes_new_job.txt
./technical/government/Media/Barnes_pro_bono.txt
./technical/government/Media/Barnes_Volunteers.txt
./technical/government/Media/Barr_sharpening_ax.txt
./technical/government/Media/BergenCountyRecord.txt
./technical/government/Media/Bias_on_the_Job.txt
./technical/government/Media/Boone_legal_service.txt
./technical/government/Media/Bridging_legal_aid_gap.txt
./technical/government/Media/BusinessWire.txt
./technical/government/Media/BusinessWire2.txt
./technical/government/Media/Butler_Co_attorneys.txt
./technical/government/Media/Campaign_Pays.txt
./technical/government/Media/City_Council_Budget.txt
./technical/government/Media/Civil_Matters.txt
./technical/government/Media/CommercialAppealMemphis2.txt
./technical/government/Media/Commercial_Appeal.txt
./technical/government/Media/Court_Keeps_Judge_From.txt
./technical/government/Media/Crains_New_York_Business.txt
./technical/government/Media/defend_yourself.txt
./technical/government/Media/Disaster_center.txt
./technical/government/Media/Do-it-yourself_divorce.txt
./technical/government/Media/Domestic_violence_aid.txt
./technical/government/Media/Domestic_Violence_Ruling.txt
./technical/government/Media/Donald_Hilliker.txt
./technical/government/Media/Entities_Merge.txt
./technical/government/Media/Eviction_law.txt
./technical/government/Media/families_saved.txt
./technical/government/Media/Federal_agency.txt
./technical/government/Media/Few_who_need.txt
./technical/government/Media/fight_domestic_abuse.txt
./technical/government/Media/Fire_Victims_Sue.txt
./technical/government/Media/Firm_to_the_Poor_Needs_Help.txt
./technical/government/Media/FortWorthStarTelegram.txt
./technical/government/Media/Free_Legal_Assistance.txt
./technical/government/Media/Free_legal_service.txt
./technical/government/Media/Funding_cuts_force.txt
./technical/government/Media/Funding_May_Limit.txt
./technical/government/Media/Funds_Shortage.txt
./technical/government/Media/FY_04_Budget_Outlook.txt
./technical/government/Media/Ginny_Kilgore.txt
./technical/government/Media/Good_guys_reward.txt
./technical/government/Media/grants_fail_to_come.txt
./technical/government/Media/Greedy_Generous.txt
./technical/government/Media/GreensburgDailyNews.txt
./technical/government/Media/Hard_to_Get.txt
./technical/government/Media/Helping_Hands.txt
./technical/government/Media/Helping_Out.txt
./technical/government/Media/help_rent-to-own_tenants.txt
./technical/government/Media/Higher_court.txt
./technical/government/Media/Higher_Registration_Fees.txt
./technical/government/Media/highlight_Senior_Day.txt
./technical/government/Media/IOLTA_INTEREST_RATE.txt
./technical/government/Media/It_Pays_to_Know.txt
./technical/government/Media/Justice_for_all.txt
./technical/government/Media/Justice_requests.txt
./technical/government/Media/Kiosks_for_court_forms.txt
./technical/government/Media/Law-school_grads.txt
./technical/government/Media/Lawyer_Web_Survey.txt
./technical/government/Media/Law_Award_from_College.txt
./technical/government/Media/Law_Schools.txt
./technical/government/Media/Legal-aid_chief.txt
./technical/government/Media/Legal_Aid_attorney.txt
./technical/government/Media/Legal_Aid_campaign.txt
./technical/government/Media/Legal_Aid_in_Clay_County.txt
./technical/government/Media/Legal_Aid_looks_to_legislators.txt
./technical/government/Media/Legal_Aid_Society.txt
./technical/government/Media/Legal_hotline.txt
./technical/government/Media/Legal_services_for_poor.txt
./technical/government/Media/Legal_system_fails_poor.txt
./technical/government/Media/less_legal_aid.txt
./technical/government/Media/Library_Lawyers.txt
./technical/government/Media/Lindsays_legacy.txt
./technical/government/Media/Local_Attorneys.txt
./technical/government/Media/Lockyer_Warns.txt
./technical/government/Media/Low-income_children.txt
./technical/government/Media/Major_Changes.txt
./technical/government/Media/Making_a_case.txt
./technical/government/Media/man_on_national_team.txt
./technical/government/Media/Marylands_Legal_Aid.txt
./technical/government/Media/New_funding_sources.txt
./technical/government/Media/New_Online_Resources.txt
./technical/government/Media/NJ_Legal_Services.txt
./technical/government/Media/Nonprofit_Buys.txt
./technical/government/Media/not_accessible_to_disabled.txt
./technical/government/Media/Oregon_Poor.txt
./technical/government/Media/Owning_a_Piece.txt
./technical/government/Media/Paralegal_Honored.txt
./technical/government/Media/Philly_Lawyers.txt
./technical/government/Media/Politician_Practices.txt
./technical/government/Media/Poor_Lacking_Legal_Aid.txt
./technical/government/Media/Poverty_Lawyers.txt
./technical/government/Media/predatory_loans.txt
./technical/government/Media/Pro-bono_road_show.txt
./technical/government/Media/Program_Lodges.txt
./technical/government/Media/Providing_Legal_Aid.txt
./technical/government/Media/pro_bono_efforts.txt
./technical/government/Media/Pro_Bono_Services.txt
./technical/government/Media/Raising_the_Bar.txt
./technical/government/Media/Rental_rules.txt
./technical/government/Media/residents_sue_city.txt
./technical/government/Media/Retirement_Has_Its_Appeal.txt
./technical/government/Media/RoanokeTimes.txt
./technical/government/Media/Rumble_in_the_Bronx.txt
./technical/government/Media/Self-Help_Website.txt
./technical/government/Media/Service_Agency.txt
./technical/government/Media/State_funding.txt
./technical/government/Media/Supporting_Legal_Center.txt
./technical/government/Media/Targeting_Domestic_Violence.txt
./technical/government/Media/Texas_Lawyer.txt
./technical/government/Media/Texas_Supreme_Court.txt
./technical/government/Media/The_Bend_Bulletin.txt
./technical/government/Media/The_Columbian.txt
./technical/government/Media/The_State_of_Pro_Bono.txt
./technical/government/Media/Too_Crucial_to_Take_Cut.txt
./technical/government/Media/Towson_Attorney.txt
./technical/government/Media/Understanding.txt
./technical/government/Media/Unusual_Woodburn.txt
./technical/government/Media/Using_Tech_Tools.txt
./technical/government/Media/Valley_Needing_Legal_Services.txt
./technical/government/Media/Volunteers_Step_Up.txt
./technical/government/Media/water_fees.txt
./technical/government/Media/Weak_economy.txt
./technical/government/Media/Wilmington_lawyer.txt
./technical/government/Media/Wingates_winds.txt
./technical/government/Media/Workers_aid_center.txt
./technical/government/Media/Working_for_Free.txt
./technical/government/Post_Rate_Comm
./technical/plos
./technical/plos/journal.pbio.0020010.txt
./technical/plos/journal.pbio.0020040.txt
./technical/plos/journal.pbio.0020047.txt
./technical/plos/journal.pbio.0020063.txt
./technical/plos/journal.pbio.0020071.txt
./technical/plos/journal.pbio.0020100.txt
./technical/plos/journal.pbio.0020105.txt
./technical/plos/journal.pbio.0020112.txt
./technical/plos/journal.pbio.0020147.txt
./technical/plos/journal.pbio.0020262.txt
./technical/plos/journal.pbio.0020263.txt
./technical/plos/journal.pbio.0020272.txt
./technical/plos/journal.pbio.0020346.txt
./technical/plos/journal.pbio.0020353.txt
./technical/plos/journal.pbio.0020430.txt
./technical/plos/journal.pbio.0030105.txt
./technical/plos/journal.pbio.0030129.txt
./technical/plos/pmed.0010022.txt
./technical/plos/pmed.0010023.txt
./technical/plos/pmed.0010024.txt
./technical/plos/pmed.0010025.txt
./technical/plos/pmed.0010026.txt
./technical/plos/pmed.0010029.txt
./technical/plos/pmed.0010030.txt
./technical/plos/pmed.0010034.txt
./technical/plos/pmed.0010041.txt
./technical/plos/pmed.0010046.txt
./technical/plos/pmed.0010047.txt
./technical/plos/pmed.0010048.txt
./technical/plos/pmed.0010049.txt
./technical/plos/pmed.0010050.txt
./technical/plos/pmed.0010051.txt
./technical/plos/pmed.0010061.txt
./technical/plos/pmed.0010067.txt
./technical/plos/pmed.0010068.txt
./technical/plos/pmed.0010069.txt
./technical/plos/pmed.0010070.txt
./technical/plos/pmed.0010071.txt
./technical/plos/pmed.0020019.txt
./technical/plos/pmed.0020020.txt
./technical/plos/pmed.0020021.txt
./technical/plos/pmed.0020022.txt
./technical/plos/pmed.0020023.txt
./technical/plos/pmed.0020024.txt
./technical/plos/pmed.0020027.txt
./technical/plos/pmed.0020028.txt
./technical/plos/pmed.0020033.txt
./technical/plos/pmed.0020035.txt
./technical/plos/pmed.0020036.txt
./technical/plos/pmed.0020047.txt
./technical/plos/pmed.0020048.txt
./technical/plos/pmed.0020055.txt
./technical/plos/pmed.0020065.txt
./technical/plos/pmed.0020074.txt
./technical/plos/pmed.0020082.txt
./technical/plos/pmed.0020085.txt
./technical/plos/pmed.0020086.txt
./technical/plos/pmed.0020088.txt
./technical/plos/pmed.0020090.txt
./technical/plos/pmed.0020091.txt
./technical/plos/pmed.0020094.txt
./technical/plos/pmed.0020098.txt
./technical/plos/pmed.0020099.txt
./technical/plos/pmed.0020104.txt
./technical/plos/pmed.0020113.txt
./technical/plos/pmed.0020114.txt
./technical/plos/pmed.0020115.txt
./technical/plos/pmed.0020116.txt
./technical/plos/pmed.0020117.txt
./technical/plos/pmed.0020118.txt
./technical/plos/pmed.0020120.txt
./technical/plos/pmed.0020144.txt
./technical/plos/pmed.0020145.txt
./technical/plos/pmed.0020146.txt
./technical/plos/pmed.0020148.txt
./technical/plos/pmed.0020149.txt
./technical/plos/pmed.0020150.txt
./technical/plos/pmed.0020155.txt
./technical/plos/pmed.0020157.txt
./technical/plos/pmed.0020158.txt
./technical/plos/pmed.0020180.txt
./technical/plos/pmed.0020181.txt
./technical/plos/pmed.0020187.txt
./technical/plos/pmed.0020189.txt
./technical/plos/pmed.0020191.txt
./technical/plos/pmed.0020192.txt
./technical/plos/pmed.0020194.txt
./technical/plos/pmed.0020195.txt
./technical/plos/pmed.0020196.txt
./technical/plos/pmed.0020197.txt
./technical/plos/pmed.0020198.txt
./technical/plos/pmed.0020200.txt
./technical/plos/pmed.0020201.txt
./technical/plos/pmed.0020203.txt
./technical/plos/pmed.0020208.txt
./technical/plos/pmed.0020226.txt
./technical/plos/pmed.0020231.txt
./technical/plos/pmed.0020235.txt
./technical/plos/pmed.0020236.txt
./technical/plos/pmed.0020237.txt
./technical/plos/pmed.0020238.txt
./technical/plos/pmed.0020239.txt
./technical/plos/pmed.0020242.txt
./technical/plos/pmed.0020257.txt
./technical/plos/pmed.0020258.txt
./technical/plos/pmed.0020268.txt
./technical/plos/pmed.0020272.txt
./technical/plos/pmed.0020273.txt
./technical/plos/pmed.0020274.txt
./technical/plos/pmed.0020275.txt
./technical/plos/pmed.0020278.txt
./technical/plos/pmed.0020281.txt
./test.sh
./TestDocSearch.class
./TestDocSearch.java
./URLHandler.class
```
The smallest file is listed at the bottom. This command is useful because it is may help with cleaning up any possibily unnecessary files that may be cluttering the directory. In this case ```./URLHandler.class``` is a necessary file.<br>
<br>
Similarly, we can use the ```find -size``` command to search for files that may be extremely large. By running the command ```find -size +100k``` is run in the working directory ```.\docsearch```, it will search each directory within ```.\docsearch``` for any files that are larger than ```100 kilobytes.```

```
darrenlov@darren:/mnt/c/Users/dlov/Desktop/cse15l/docsearch$ find -size +100k
./lib/junit-4.13.2.jar
./technical/911report/chapter-1.txt
./technical/911report/chapter-12.txt
./technical/911report/chapter-13.2.txt
./technical/911report/chapter-13.3.txt
./technical/911report/chapter-13.4.txt
./technical/911report/chapter-13.5.txt
./technical/911report/chapter-3.txt
./technical/911report/chapter-6.txt
./technical/911report/chapter-7.txt
./technical/911report/chapter-9.txt
./technical/biomed/1471-2105-3-2.txt
./technical/government/About_LSC/commission_report.txt
./technical/government/About_LSC/State_Planning_Report.txt
./technical/government/Env_Prot_Agen/bill.txt
./technical/government/Env_Prot_Agen/ctm4-10.txt
./technical/government/Env_Prot_Agen/multi102902.txt
./technical/government/Env_Prot_Agen/tech_adden.txt
./technical/government/Gen_Account_Office/ai9868.txt
./technical/government/Gen_Account_Office/d01376g.txt
./technical/government/Gen_Account_Office/d01591sp.txt
./technical/government/Gen_Account_Office/d0269g.txt
./technical/government/Gen_Account_Office/d02701.txt
./technical/government/Gen_Account_Office/gg96118.txt
./technical/government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
./technical/government/Gen_Account_Office/im814.txt
./technical/government/Gen_Account_Office/May1998_ai98068.txt
./technical/government/Gen_Account_Office/pe1019.txt
./technical/government/Gen_Account_Office/Sept27-2002_d02966.txt
./technical/government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
```
This command, in the context of a larger project, is useful because it could be used to find large files, which may be important files with lots of intricate code. In this case, we can see that ```./lib/junit-4.13.2.jar``` was found with the command, which is an important part to help with testing our program. It also may be used to find files that are taking up excessive space.

# Sources
[^1]: ChatGPT
[^2]: [Linux Manual Page](https://man7.org/linux/man-pages/man1/find.1.html)

ChatGPT Prompts:
- how do i use ```find -type```
