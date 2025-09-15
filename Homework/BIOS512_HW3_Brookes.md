---
output:
  pdf_document: default
  html_document: default
---
# Homework 03 - Nonstandard Evaluation and Git

## Nonstandard Evaluation


### Question 1
Imagine we have a data frame called `data`, with a `type` column. Which one works and why?\
Function 1:
```
group_and_tally <- function(df, column){
    df %>% group_by({{ column }}) %>% tally();
}
group_and_tally(data, type);
```

Function 2:
```
group_and_tally <- function(df, column){
    df %>% group_by(column) %>% tally();
}
group_and_tally(data, type);
```
### Q1 Answer
<span style="color:purple">Function 1 works because it includes the embrace symbols `{{ }}`. The embrace symbols are needed in tidyverse because otherwise the funciton will try to process `column` as an object instead of looking for the column name `type` (which is what is happening in Funciton 2).<span>

## Git
For the questions below, please add the commands you used to complete these steps.

### Question 2
Set up your git repo on your local computer. If you already make a git repo on GitHub, but it isn’t on your local computer - clone it.
mkdir -p ~/git/BIOS512
cd ~/git/BIOS512
cat <<EOF > README.md #BIOS512 Course 
    This is for the BIOS512 Course, Fall 2025. 
    author: Jama-Brookes
        EOF
git init
git add README.md
git commit -m "First commit"
git branch -M main
git remote add origin git@github.com:Jama-Brookes/BIOS512.git
git push -u origin main
### Question 3
Set up your SSH key.
cd /tmp/
mkdir ssh-keys
cd ssh-keys/
ssh-keyge
cat ~/.ssh/id_ed25519.pub
<span style="color:purple">Then this SSH key was copied added to Git Hub via Settings > SSH and GPG keys.<span>

### Question 4
a) Add a HW2 directory to your git repo through the terminal with a HW.md file that says "This is for homework 2."

<span style="color:purple">In my BIOS512 git repo from my terminal, I added the following code:<span>
mkdir HW2 
cd HW2
echo "This is for homework 2." > HW2.md
b) *Add* HW2.md to the staging area. Then, use the command to see which files have been modified, staged for commit, or are untracked. What does it show?
They should copy paste the terminal response after git status, and show that key used the commands below.
git add HW2.md
git status
<span style="color:purple">Status showed:<span>
```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   HW2.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   ../Homework/BIOS512_HW3_Brookes.ipynb

```

c) Save file changes to the main branch.
git commit -m "Add HW2 folder and HW2.md file"
git push -u origin main
d) Now, edit the HW2.md file to give it a title.
cat <<EOF > HW2.md
# Homework 2
Example of editing documents in Git in HW3.
EOF
e) Use the command that compares current, unsaved changes to the main branch. What does it say?\

<span style="color:purple">Command:<span>
    
`git diff`

<span style="color:purple">Output:<span>
diff --git a/HW2/HW2.md b/HW2/HW2.md
index 1a010d3..2eaef26 100644
--- a/HW2/HW2.md
+++ b/HW2/HW2.md
@@ -1 +1,2 @@
-This is for homework 2.
+# Homework 2
+Example of editing documents in Git in HW3.
f) Use the command that checks the status of the working directory and the staging area *again*. What does it say?
<span style="color:purple">Command:<span>
    
`git status`

<span style="color:purple">Output:<span>
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   HW2.md
	modified:   ../Homework/.ipynb_checkpoints/BIOS512_HW3_Brookes-checkpoint.ipynb
	modified:   ../Homework/BIOS512_HW3_Brookes.ipynb

no changes added to commit (use "git add" and/or "git commit -a")
g) Once again, add HW2.md to the staging area and save the file changes to the main branch. Then, get use the command that gives you project history and paste the output in your homework.

<span style="color:purple">Commands:<span>
    
`git add HW2.md`

`git commit -m "Updated HW2.md with a Title"`

`git push`

`git log`

<span style="color:purple">Output:<span>

commit 94454d90ac235a3054c7ee1a92689f12bbf763c0 (HEAD -> main, origin/main)
Author: Jama Brookes <brookesjj@Mac.lan>
Date:   Tue Sep 9 17:11:20 2025 -0400

    Updated HW2.md with a Title

commit 584b86f809f79f4a97968926035b79dea37eb2d2
Author: Jama Brookes <brookesjj@Mac.lan>
Date:   Tue Sep 9 16:24:38 2025 -0400

    Add HW2 folder and HW2.md file

commit 794a793d3187f32d0837fe93055a5d0c14549976
Author: Jama Brookes <brookesjj@Mac.lan>
Date:   Tue Sep 9 16:08:46 2025 -0400

    Add homework folder

commit 4115c83e72013300cf8afe0988a419295ddb17de
Author: Jama Brookes <brookesjj@Mac.lan>
Date:   Tue Sep 9 15:14:58 2025 -0400

    First commit
h) Do some searching... What `git` command will provide you documentation on other commands? Use that command to find documentation on `git log` and `git show`. What does `--since` mean in regards to `git log`? Copy and paste what is written in the documentation.

<span style="color:purple">Command:<span>
    
`git log --help`

`git show --help`

<span style="color:purple">Output:<span>
--since=<date>, --after=<date>
        Show commits more recent than <date>.
## Tidyverse

Note: Please make sure Binder is set up correctly to run this section. You can follow the instructions here: https://github.com/rjenki/BIOS512. 

**Please show your code for this section!** Before completing this section, please run the following.


```R
library(tidyverse)
if (!dir.exists("intermediate")) dir.create("intermediate", recursive = TRUE)
if (!exists("mdpre")) mdpre <- function(x) { print(x) }
if (!exists("ggmd"))  ggmd  <- function(p) { print(p) }
```

### Question 5

Download the patient_names.csv and patient_properties.csv files from Canvas and read them into R. Manually set the date columns to be date variables. Print the first 10 observations of each.


```R
patient_names <- read.csv(file = "/Users/brookesjj/git/BIOS512/data/patient_names.csv")
patient_properties <- read.csv(file = "/Users/brookesjj/git/BIOS512/data/patient_properties.csv")
#changing date columns to date variables)
patient_names$BIRTHDATE <- as.Date(patient_names$BIRTHDATE, "%m/%d/%y")
patient_names$DEATHDATE <- as.Date(patient_names$DEATHDATE, "%m/%d/%y")
head(patient_names, n = 10)
head(patient_properties, n = 10)

```


<table class="dataframe">
<caption>A data.frame: 10 × 7</caption>
<thead>
	<tr><th></th><th scope=col>ID</th><th scope=col>BIRTHDATE</th><th scope=col>DEATHDATE</th><th scope=col>FIRST</th><th scope=col>LAST</th><th scope=col>CITY</th><th scope=col>STATE</th></tr>
	<tr><th></th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>1977-03-19</td><td>NA</td><td>Nikita578 </td><td>Erdman779     </td><td>Quincy   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>2</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>2040-02-19</td><td>NA</td><td>Zane918   </td><td>Hodkiewicz467 </td><td>Boston   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>3</th><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>2058-06-04</td><td>NA</td><td>Quinn173  </td><td>Marquardt819  </td><td>Quincy   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>4</th><td>770518e4-6133-648e-60c9-071eb2f0e2ce</td><td>2028-12-25</td><td>2017-09-29</td><td>Abel832   </td><td>Smitham825    </td><td>Boston   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>5</th><td>f96addf5-81b9-0aab-7855-d208d3d352c5</td><td>2028-12-25</td><td>2014-02-23</td><td>Edwin773  </td><td>Labadie908    </td><td>Boston   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>6</th><td>8e9650d1-788a-78f9-4a28-d08f7f95354a</td><td>2028-12-25</td><td>NA</td><td>Frankie174</td><td>Oberbrunner298</td><td>Boston   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>7</th><td>183df435-4190-060e-8f8e-bf63c572b266</td><td>2057-11-08</td><td>NA</td><td>Eilene124 </td><td>Walsh511      </td><td>Cambridge</td><td>Massachusetts</td></tr>
	<tr><th scope=row>8</th><td>720560d4-51da-c38c-ee90-c15935278df1</td><td>1972-06-27</td><td>NA</td><td>Lowell343 </td><td>Price929      </td><td>Quincy   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>9</th><td>217851b0-5f47-d376-18b9-0fe4ba77207e</td><td>2054-03-06</td><td>NA</td><td>Adrian111 </td><td>Gleason633    </td><td>Boston   </td><td>Massachusetts</td></tr>
	<tr><th scope=row>10</th><td>ff331e5c-ab16-e218-f39a-63e11de1ed75</td><td>2027-07-10</td><td>NA</td><td>Eugene421 </td><td>Abernathy524  </td><td>Boston   </td><td>Massachusetts</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A data.frame: 10 × 3</caption>
<thead>
	<tr><th></th><th scope=col>ID</th><th scope=col>property</th><th scope=col>value</th></tr>
	<tr><th></th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>MARITAL  </td><td>M          </td></tr>
	<tr><th scope=row>2</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>RACE     </td><td>white      </td></tr>
	<tr><th scope=row>3</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>ETHNICITY</td><td>nonhispanic</td></tr>
	<tr><th scope=row>4</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>GENDER   </td><td>F          </td></tr>
	<tr><th scope=row>5</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>MARITAL  </td><td>M          </td></tr>
	<tr><th scope=row>6</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>RACE     </td><td>white      </td></tr>
	<tr><th scope=row>7</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>ETHNICITY</td><td>nonhispanic</td></tr>
	<tr><th scope=row>8</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>GENDER   </td><td>M          </td></tr>
	<tr><th scope=row>9</th><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>MARITAL  </td><td>M          </td></tr>
	<tr><th scope=row>10</th><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>RACE     </td><td>white      </td></tr>
</tbody>
</table>



### Question 6
In the data frame pulled from patient_properties, you'll notice that the data is long, not wide. Do a pivot to make the properties their own columns. Print the first 10 observations after you do so.


```R
properties_wide <- patient_properties %>% 
                        pivot_wider(id_cols = ID,
                                    names_from = property,
                                    values_from = value)
head(properties_wide, n = 10)
```


<table class="dataframe">
<caption>A tibble: 10 × 5</caption>
<thead>
	<tr><th scope=col>ID</th><th scope=col>MARITAL</th><th scope=col>RACE</th><th scope=col>ETHNICITY</th><th scope=col>GENDER</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>M</td><td>white </td><td>nonhispanic</td><td>F</td></tr>
	<tr><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><td>770518e4-6133-648e-60c9-071eb2f0e2ce</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><td>f96addf5-81b9-0aab-7855-d208d3d352c5</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><td>8e9650d1-788a-78f9-4a28-d08f7f95354a</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><td>183df435-4190-060e-8f8e-bf63c572b266</td><td>M</td><td>asian </td><td>nonhispanic</td><td>F</td></tr>
	<tr><td>720560d4-51da-c38c-ee90-c15935278df1</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><td>217851b0-5f47-d376-18b9-0fe4ba77207e</td><td>S</td><td>black </td><td>hispanic   </td><td>M</td></tr>
	<tr><td>ff331e5c-ab16-e218-f39a-63e11de1ed75</td><td>M</td><td>native</td><td>hispanic   </td><td>M</td></tr>
</tbody>
</table>



### Question 7
Perform a left join of the names and properties_wide data frames by the ID column and print the first 10 rows.


```R
patients_left <- patient_names %>% left_join(properties_wide,
                              by = "ID")
head(patients_left, n=10)
```


<table class="dataframe">
<caption>A data.frame: 10 × 11</caption>
<thead>
	<tr><th></th><th scope=col>ID</th><th scope=col>BIRTHDATE</th><th scope=col>DEATHDATE</th><th scope=col>FIRST</th><th scope=col>LAST</th><th scope=col>CITY</th><th scope=col>STATE</th><th scope=col>MARITAL</th><th scope=col>RACE</th><th scope=col>ETHNICITY</th><th scope=col>GENDER</th></tr>
	<tr><th></th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>1977-03-19</td><td>NA</td><td>Nikita578 </td><td>Erdman779     </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>F</td></tr>
	<tr><th scope=row>2</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>2040-02-19</td><td>NA</td><td>Zane918   </td><td>Hodkiewicz467 </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>3</th><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>2058-06-04</td><td>NA</td><td>Quinn173  </td><td>Marquardt819  </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>4</th><td>770518e4-6133-648e-60c9-071eb2f0e2ce</td><td>2028-12-25</td><td>2017-09-29</td><td>Abel832   </td><td>Smitham825    </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>5</th><td>f96addf5-81b9-0aab-7855-d208d3d352c5</td><td>2028-12-25</td><td>2014-02-23</td><td>Edwin773  </td><td>Labadie908    </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>6</th><td>8e9650d1-788a-78f9-4a28-d08f7f95354a</td><td>2028-12-25</td><td>NA</td><td>Frankie174</td><td>Oberbrunner298</td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>7</th><td>183df435-4190-060e-8f8e-bf63c572b266</td><td>2057-11-08</td><td>NA</td><td>Eilene124 </td><td>Walsh511      </td><td>Cambridge</td><td>Massachusetts</td><td>M</td><td>asian </td><td>nonhispanic</td><td>F</td></tr>
	<tr><th scope=row>8</th><td>720560d4-51da-c38c-ee90-c15935278df1</td><td>1972-06-27</td><td>NA</td><td>Lowell343 </td><td>Price929      </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>9</th><td>217851b0-5f47-d376-18b9-0fe4ba77207e</td><td>2054-03-06</td><td>NA</td><td>Adrian111 </td><td>Gleason633    </td><td>Boston   </td><td>Massachusetts</td><td>S</td><td>black </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>10</th><td>ff331e5c-ab16-e218-f39a-63e11de1ed75</td><td>2027-07-10</td><td>NA</td><td>Eugene421 </td><td>Abernathy524  </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>native</td><td>hispanic   </td><td>M</td></tr>
</tbody>
</table>



### Question 8
Notice something interesting about the names in our data set. Fix the name formatting and print the first 10 observations.


```R
patients_left$FIRST <- gsub("[0-9]", "", patients_left$FIRST)
patients_left$LAST <- gsub("[0-9]", "", patients_left$LAST)
head(patients_left, 10)
```


<table class="dataframe">
<caption>A data.frame: 10 × 11</caption>
<thead>
	<tr><th></th><th scope=col>ID</th><th scope=col>BIRTHDATE</th><th scope=col>DEATHDATE</th><th scope=col>FIRST</th><th scope=col>LAST</th><th scope=col>CITY</th><th scope=col>STATE</th><th scope=col>MARITAL</th><th scope=col>RACE</th><th scope=col>ETHNICITY</th><th scope=col>GENDER</th></tr>
	<tr><th></th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;date&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>5605b66b-e92d-c16c-1b83-b8bf7040d51f</td><td>1977-03-19</td><td>NA</td><td>Nikita </td><td>Erdman     </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>F</td></tr>
	<tr><th scope=row>2</th><td>6e5ae27c-8038-7988-e2c0-25a103f01bfa</td><td>2040-02-19</td><td>NA</td><td>Zane   </td><td>Hodkiewicz </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>3</th><td>8123d076-0886-9007-e956-d5864aa121a7</td><td>2058-06-04</td><td>NA</td><td>Quinn  </td><td>Marquardt  </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>4</th><td>770518e4-6133-648e-60c9-071eb2f0e2ce</td><td>2028-12-25</td><td>2017-09-29</td><td>Abel   </td><td>Smitham    </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>5</th><td>f96addf5-81b9-0aab-7855-d208d3d352c5</td><td>2028-12-25</td><td>2014-02-23</td><td>Edwin  </td><td>Labadie    </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>6</th><td>8e9650d1-788a-78f9-4a28-d08f7f95354a</td><td>2028-12-25</td><td>NA</td><td>Frankie</td><td>Oberbrunner</td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>white </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>7</th><td>183df435-4190-060e-8f8e-bf63c572b266</td><td>2057-11-08</td><td>NA</td><td>Eilene </td><td>Walsh      </td><td>Cambridge</td><td>Massachusetts</td><td>M</td><td>asian </td><td>nonhispanic</td><td>F</td></tr>
	<tr><th scope=row>8</th><td>720560d4-51da-c38c-ee90-c15935278df1</td><td>1972-06-27</td><td>NA</td><td>Lowell </td><td>Price      </td><td>Quincy   </td><td>Massachusetts</td><td>M</td><td>white </td><td>nonhispanic</td><td>M</td></tr>
	<tr><th scope=row>9</th><td>217851b0-5f47-d376-18b9-0fe4ba77207e</td><td>2054-03-06</td><td>NA</td><td>Adrian </td><td>Gleason    </td><td>Boston   </td><td>Massachusetts</td><td>S</td><td>black </td><td>hispanic   </td><td>M</td></tr>
	<tr><th scope=row>10</th><td>ff331e5c-ab16-e218-f39a-63e11de1ed75</td><td>2027-07-10</td><td>NA</td><td>Eugene </td><td>Abernathy  </td><td>Boston   </td><td>Massachusetts</td><td>M</td><td>native</td><td>hispanic   </td><td>M</td></tr>
</tbody>
</table>



### Question 9
Using a for statement to loop through the categorical variables (excluding name and ID), print the counts of each unique value in descending order, using the mdpre() function for formatting.


```R
#definding mdpre

pat_charact <- subset(patients_left, select = MARITAL:GENDER)
#patients_left %>% group_by(MARITAL) %>% tally()


group_and_tally <- function(df, column){
  df %>% 
    group_by({{ column }}) %>% 
      tally()
}

for (i in colnames(pat_charact)) {
  cat("\nCounts for column:", i, "\n")
  mdpre(group_and_tally(patients_left, !!sym(i)))
}
```

    
    Counts for column: MARITAL 
    [90m# A tibble: 5 × 2[39m
      MARITAL     n
      [3m[90m<chr>[39m[23m   [3m[90m<int>[39m[23m
    [90m1[39m Fine        1
    [90m2[39m M         782
    [90m3[39m S         189
    [90m4[39m male        1
    [90m5[39m [31mNA[39m          1
    
    Counts for column: RACE 
    [90m# A tibble: 7 × 2[39m
      RACE         n
      [3m[90m<chr>[39m[23m    [3m[90m<int>[39m[23m
    [90m1[39m asian       90
    [90m2[39m asiann       1
    [90m3[39m black      163
    [90m4[39m hawaiian    13
    [90m5[39m native      11
    [90m6[39m other       16
    [90m7[39m white      680
    
    Counts for column: ETHNICITY 
    [90m# A tibble: 4 × 2[39m
      ETHNICITY       n
      [3m[90m<chr>[39m[23m       [3m[90m<int>[39m[23m
    [90m1[39m hispani         1
    [90m2[39m hispanic      190
    [90m3[39m nonhispani      2
    [90m4[39m nonhispanic   781
    
    Counts for column: GENDER 
    [90m# A tibble: 5 × 2[39m
      GENDER     n
      [3m[90m<chr>[39m[23m  [3m[90m<int>[39m[23m
    [90m1[39m F        478
    [90m2[39m Female     1
    [90m3[39m M        493
    [90m4[39m Male       1
    [90m5[39m female     1


### Question 10
If you see any weird values, get rid of the ones that don't make sense, and combine the ones that are formatted wrong. Don't forget ot check the dates! Print the new tables for categorical values, and print the date ranges.


```R
#noticing that some ages are incorrectly, so fixing this
patients_left$BIRTHDATE <- as.Date(ifelse(patients_left$BIRTHDATE > Sys.Date(), 
  format(patients_left$BIRTHDATE, "19%y-%m-%d"), 
  format(patients_left$BIRTHDATE)))
patients_left$DEATHDATE <- as.Date(ifelse(patients_left$DEATHDATE > Sys.Date(), 
  format(patients_left$DEATHDATE, "19%y-%m-%d"), 
  format(patients_left$DEATHDATE)))

#removing values that do not make sense for MARITAL (aka, not M or S)
patients_left$MARITAL <- ifelse(patients_left$MARITAL %in% c("M", "S"), 
                                patients_left$MARITAL, NA)
patients_left <- patients_left %>% mutate(MARITAL = recode(MARITAL,
                            M = "Married",
                            S = "Single"))
                                          
#fixing "asiann" to be "asian" in the RACE variable
patients_left$RACE <- ifelse(patients_left$RACE == "asiann", 
                              "asian", patients_left$RACE)

#fixing Ethnicity column values of hispani and nonhispani
patients_left <- patients_left %>% mutate(ETHNICITY = case_when(
                                                    ETHNICITY %in% c("hispani", "hispanic") ~ "Hispanic",
                                                    ETHNICITY %in% c("nonhispani", "nonhispanic") ~ "Nonhispanic",
                                                    TRUE ~ ETHNICITY)
                                         )

#fixing Gender columns to all be "Female" or "Male"
patients_left <- patients_left %>% mutate(GENDER = case_when(
                                                    GENDER %in% c("F", "Female", "female") ~ "Female",
                                                    GENDER %in% c("M", "Male") ~ "Male",
                                                    TRUE ~ GENDER)
                                         )

for (i in colnames(pat_charact)) {
  cat("\nCounts for column:", i, "\n")
  mdpre(group_and_tally(patients_left, !!sym(i)))
}

```

    
    Counts for column: MARITAL 
    [90m# A tibble: 3 × 2[39m
      MARITAL     n
      [3m[90m<chr>[39m[23m   [3m[90m<int>[39m[23m
    [90m1[39m Married   782
    [90m2[39m Single    189
    [90m3[39m [31mNA[39m          3
    
    Counts for column: RACE 
    [90m# A tibble: 6 × 2[39m
      RACE         n
      [3m[90m<chr>[39m[23m    [3m[90m<int>[39m[23m
    [90m1[39m asian       91
    [90m2[39m black      163
    [90m3[39m hawaiian    13
    [90m4[39m native      11
    [90m5[39m other       16
    [90m6[39m white      680
    
    Counts for column: ETHNICITY 
    [90m# A tibble: 2 × 2[39m
      ETHNICITY       n
      [3m[90m<chr>[39m[23m       [3m[90m<int>[39m[23m
    [90m1[39m Hispanic      191
    [90m2[39m Nonhispanic   783
    
    Counts for column: GENDER 
    [90m# A tibble: 2 × 2[39m
      GENDER     n
      [3m[90m<chr>[39m[23m  [3m[90m<int>[39m[23m
    [90m1[39m Female   480
    [90m2[39m Male     494


### Question 11
Make a histogram of the ages of patients by gender. 


```R
#creating age variable

patients_left <- patients_left %>%
                    mutate(AGE = round(ifelse(!is.na(patients_left$DEATHDATE), 
                                     as.numeric(patients_left$DEATHDATE - patients_left$BIRTHDATE)/365, 
                                     as.numeric(Sys.Date() - patients_left$BIRTHDATE)/365)
                           ))
#checking age variable
#head(patients_left, 10)

#creating histogram
library(ggplot2)

patients_left %>% ggplot(aes(x=AGE, fill = GENDER)) + 
  geom_histogram(bins = 15) +
    theme_bw() +
     labs(x="Age",y="Count of Participants",title="Age Distribution") +
     theme(plot.title = element_text(hjust = 0.5))


```


    
![png](BIOS512_HW3_Brookes_files/BIOS512_HW3_Brookes_40_0.png)
    


### Question 12
Make a scatterplot of birthdate by martial status.


```R
patients_left %>% ggplot(aes(x=BIRTHDATE, y= MARITAL, color = MARITAL)) + 
  geom_jitter() +
    theme_bw() +
     labs(x="Birthdate",y="Marital Status",title="Date of Birth by Marital Status") +
     theme(plot.title = element_text(hjust = 0.5))
```


    
![png](BIOS512_HW3_Brookes_files/BIOS512_HW3_Brookes_42_0.png)
    



```R
convert_ipynb("./BIOS512_HW3_Brookes.ipynb", "./BIOS512_HW3_Brookes.Rmd" = xfun::with_ext(input, "Rmd"))
```


    Error in convert_ipynb("./BIOS512_HW3_Brookes.ipynb", `./BIOS512_HW3_Brookes.Rmd` = xfun::with_ext(input, : could not find function "convert_ipynb"
    Traceback:




```R

```
