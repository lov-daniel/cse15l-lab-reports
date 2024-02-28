# Lab 4 (Vim)

## Step 4 (Logging Into SSH)

First, I open the command line terminal on my Windows machine which is ```Windows PowerShell```.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/ea5abcb2-46d5-40ec-bc91-705c3250e681)

In order to log into ```SSH```, I use my login credentials which in this case is simply ```dlov@ieng6.ucsd.edu``` since I have my public key stored inside of the ```ieng6``` machine. My screen looks like the following:

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/cfdce9f9-c6c2-4e32-aedb-9a7de5a7c56d)
The keystrokes used in this step were ```ssh <SPACE> dlov@ieng6.ucsd.edu <ENTER>```. After these keystrokes were inputted, I was able to successfully log into the ```ieng6``` machine.
## Step 5 (Forking a Repository)
I opened the link to the repository and I created a fork, which can be seen from the following screenshots.
![Screenshot 2024-02-26 221612](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/a61773ce-c0da-4c39-8bd6-36c889f27217)
 - Clicking on the fork button in the top right leads into the next screenshot.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/46b3b87d-9b4e-4eda-9bdd-5cf383702e22)
 - At this page, we are able to name our fork to whatever we would like. In this case, I named the repository ```lab7fork```
 - The keystrokes used in this step were for the naming the fork of the lab: ```lab7fork```.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/2318e0ea-e2a9-4f6f-889d-244f60a15cb5)
 - After successfully forking the repositiory, we are led to the GitHub page where we can use the URL to clone.
## Step 5.5 (Cloning a Forked Repository)
Since we have an ```SSH``` key linked to GitHub, we can use the ```SSH clone url``` which can be found here:
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/072bb6c7-7195-49cf-9310-638577d38970)
 - The link was highlighted and copied to my clipboard using ```<CTRL-C>```.

Going back to the command line terminal, we can begin cloning the repository into the ```ieng6``` machine.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/68c90c61-844a-4162-931b-84c2d6fbd751)
 - The keystrokes used in this image were ```git <SPACE> clone <CTRL+V> <ENTER>``` followed by ```ls <ENTER>``` in order to confirm that the repository was properly cloned into the ```ieng6``` machine.
 - The current working directory is ```./```, meaning that the repository was cloned into the root of the ```ieng6``` machine.
 - After these keystrokes, every directory in the root was printed and we can see that a new directory was created in the root named ```lab7fork```, meaning that the ```git clone``` was successful.

To prepare for the next step, we can move into the directory that we had just cloned:
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/7dd9c306-eb15-4c4f-a966-fd4ee94ede69)
 - The keystrokes used to move into the directory was ```cd <SPACE> lab7f <TAB> <ENTER>``` followed by ```ls <ENTER>``` in order to list the finals in the current working directory, which was ```./lab7fork```.
 - ```<TAB>``` was used to auto-complete, which resulted in the full command of ```cd lab7fork/```.
 - After this command, the current working directory is ```./lab7fork```

## Step 6 (Demonstrating Failing Tests)
Now that our working directory is ```./lab7fork```, we can run the command ```bash test.sh``` in order to run the script which contains the ```JUnit``` commands.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/83d136ad-eaf7-4330-bfd3-21d9b335b266)
 - The keystrokes used were ```bash <SPACE> test.sh <ENTER>```. After these keystrokes were inputted, we were able to run the ```bash``` script, which contains commands to compile all the ```java``` files in the directory and then run the methods within ```./ListExamplesTest.java```. This resulted in a failed ```JUnit``` test as a result of a bug in ```./ListExamples.java```.

## Step 7 (Fixing The Code File)
We know that the issue is in ```./ListExamples.java```. We can run the ```vim``` command in order to make changes to the file.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/ef3dd660-7d2f-423e-b4b6-c55c0c2fd4db)
 - The keystrokes used were ```vim <SPACE> List <TAB> .java <ENTER>```.
 - The tab was used to autofill ```ListExamples```.
 - After these keystores were inputted, we entered ```vim``` and were able to start editing the file ```./ListExamples.java```.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/1f659461-1565-449a-b933-fbd13f5340d2)
 - Once we are inside of ```vim``` we can begin editing the file, ```./ListExamples.java```. Since we know that the edit we will have to make is to change the last ```index1``` to ```index2```, we can use the following commands in sequence.
 - ```?index1 <ENTER>```, which searches for the keyword ```index1``` starting from the bottom of the text file. We press ```<ENTER>``` to stop the search after the first instance from the bottom is found.
 - ```<CTRL+A>```, which increments the integer selected, in this case it is the ```1``` in  ```index1```, resulting in it becoming ```index2```.
 - ```:wq <ENTER>```, which writes and quits to the file, allowing the changes made to be saved.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/12b319e0-11f1-410b-92ff-66efce90252c)
 - Now that the changes are saved, we can proceed to the next step where we can see our changes be reflected with the tests.

## Step 8 (Demonstrating Passing Tests)
After exiting ```vim```, our working directory does not change, so we are still located at ```./lab7fork```. This means we can type the command ```bash <SPACE> test.sh <ENTER>``` in order to run our ```JUnit``` tests again.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/de3bbec3-4cf3-4e4f-92cd-82e59d92af99)
 - After the changes we made in ```./ListExamples.java``` using ```vim```, we can see that our tests pass now.
 - The keystrokes used were ```bash <SPACE> test.sh <ENTER>```. After these keystrokes were inputted, we were able to run the ```bash``` script, which contains commands to compile all the ```java``` files in the directory and then run the methods within ```./ListExamplesTest.java```. This resulted in a passed ```JUnit``` test since we have fixed the bug in ```./ListExamples.java```.

## Step 9 (Commiting And Pushing Changes)
Since we are happy with the changes made now, we can start committing and pushing the modified file, ```./ListExamples.java``` into the forked repository that we have made.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/1bca331a-640e-4283-a18d-82e48dd14d0c)
 - While still in the working directory ```./lab7fork```, we type ```git add ListExamples.java <ENTER>``` in order to add our changes in ```./ListExamples.java``` to the commit history.
 - Next, ```git <SPACE> commit <ENTER>``` in order to actually commit our changes, which prompts a commit message.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/16ebf4fe-e1a0-4ae1-acc3-e030cdd9c4b3)
 - Here, we can edit a commit message, which we can use to describe what was changed.
 - ```i``` was first used to enter ```insertion mode```.
 - ```fixed <SPACE> ListExamples.java``` was then typed as the commit message.
 - ```<ESC>``` was used to exit ```insertion mode```.
 - ```:wq <ENTER>``` was then used to write and exit the commit file.

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/c538be4d-3aa5-4203-9393-e1c03884b21f)
 - In order to have the changes we have made in the ```ieng6``` machine reflect on the fork repository we have made, we have to type the following command.
 - ```git <SPACE> push <ENTER>```, which pushes all of our committed changes into the GitHub repository.

If we check the repository on GitHub, we can see our commit message, indicating our change went through.
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/32f7b5e5-f321-4ebc-a6af-e825896d3f1a)
