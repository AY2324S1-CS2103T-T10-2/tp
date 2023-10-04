---
layout: page
title: Draft User Guide
---

---

Do you have trouble managing contacts?

Don't worry, CoordiMate is here to help!

CoordiMate is a **desktop app for event planners**. It helps you **manage your contacts and tasks** for your events, so that you can focus on the event itself.

CoordiMate is **optimized for use via a Command Line Interface (CLI)** while still having the benefits of a Graphical User Interface (GUI).

If you can type fast, CoordiMate can help you get your contact management tasks done **faster than traditional GUI apps**.

This user guide contains all the information you need to get started with CoordiMate.

<h2> Table of Contents </h2>
* Table of Contents
{:toc}

---

<div style="page-break-after: always;"></div>

## Features

1. CRUD Person
2. Find Person
3. CRUD Task
4. Find Task
5. Auto-save/load to/from disk
    1. CoordiMate automatically saves its data as a JSON file located at `[JAR file location]/data/addressbook.json`.
    2. There is no need to save manually.
    3. On startup, CoordiMate will automatically load existing data (if any) from the JSON file.
6. Editable file format
   1. Advanced users are welcome to update data directly by editing that data file.

{% include admonition.html type="danger" title="Potentially Dangerous Operation!" body="

If your changes to the data file makes its format invalid, AddressBook will discard all data and start with an empty data file at the next run.

Always make a backup before you edit!

" %}


<div style="page-break-after: always;"></div>


---


## Usage


{% include admonition.html type="info" title="Info: About Command Formats" body="

<ul>
  <li>
    <p>
      Words in <code>UPPER_CASE</code> are the parameters to be supplied by the user.<br>
      e.g. in <code>add n/NAME</code>, <code>NAME</code> is a parameter which can be used as <code>add n/John Doe</code>, <br>
      where <code>John Doe</code> is the value of the parameter <code>NAME</code>. <br>
    </p>
  </li>

  <li>
    <p>
      Items in square brackets are optional.<br>
      e.g <code>n/NAME [t/TAG]</code> can be used as <code>n/John Doe t/friend</code> or as <code>n/John Doe</code>.<br>
      Note that the square brackets (<code>[</code> and <code>]</code>) are not part of the syntax.
    </p>
  </li>

  <li>
    <p>
      Items in square brackets with <code>…</code> after them can be used multiple times, including zero times.<br>
      e.g. <code>[t/TAG]…​</code> can be used as <code> </code> (i.e. 0 times), <code>t/friend</code>, <code>t/friend t/family</code> etc.
    </p>
  </li>

  <li>
    <p>
      Parameters can be in any order.<br>
      e.g. if the command specifies <code>n/NAME p/PHONE_NUMBER</code>, <code>p/PHONE_NUMBER n/NAME</code> is also acceptable.
    </p>
  </li>

  <li>
    <p>
      Extraneous parameters for commands that do not take in parameters (such as <code>help</code>, <code>list</code>, <code>exit</code> and <code>clear</code>) will be ignored.<br>
      e.g. if the command specifies <code>help 123</code>, it will be interpreted as <code>help</code>.
    </p>
  </li>
</ul>

" %}

{% include admonition.html type="warning" title="Warning: Using the PDF version of this document" body="

If you are using a PDF version of this document, be careful when copying and pasting commands that span multiple lines as space characters surrounding line-breaks may be omitted when copied over to the application.

" %}

### 1. Viewing help : `help`

You can access the help page at any time, ensuring that you will never be lost.

Format:

```
help
```

Examples:

- `help`

Output:

- `Opened help window`
  ![help message](images/helpMessage.png)

<div style="page-break-after: always;"></div>


### 2. Adding a person: `addPerson`

Add new people that you meet, be it new clients, vendors or friends.

Format:

```
addPerson n/NAME p/PHONE_NUMBER e/EMAIL a/ADDRESS [t/TAG]…
```

{% include admonition.html type="info" title="A person can have any number of tags (including 0)." %}

Examples:

- `addPerson n/John Doe p/98765432 e/johnd@example.com a/John street, block 123, #01-01`
- `addPerson n/Betsy Crowe t/friend e/betsycrowe@example.com a/Newgate Prison p/1234567 t/criminal`

Output:

![addPerson success](images/addPerson_success.png)

Errors:

![addPerson error](images/error/addPerson_error.png)

<div style="page-break-after: always;"></div>


### 3. Listing all persons : `listPerson`

Presents you with a comprehensive list of contacts in your contact list.

Format:

```
listPerson
```

Examples:

- `listPerson`

Output:

![listPerson success](images/listPerson_success.png)

<div style="page-break-after: always;"></div>


### 4. Editing a person : `editPerson`

Enables you to make edits to an existing contact in your contact list.

Format:

```
editPerson INDEX [n/NAME] [p/PHONE] [e/EMAIL] [a/ADDRESS] [t/TAG]…
```

- Edits the person at the specified `INDEX`. The index refers to the index number shown in the displayed person list. The index **_must be a positive integer_** 1, 2, 3, …
- At least one of the optional fields must be provided.
- Existing values will be updated to the input values.
- When editing tags, the existing tags of the person will be removed i.e adding of tags is not cumulative.
- You can remove all the person’s tags by typing `t/` without specifying any tags after it.

Examples:

- `editPerson 1 p/91234567 e/johndoe@example.com` Edits the phone number and email address of the 1st person to be `91234567` and `johndoe@example.com` respectively.
- `editPerson 2 n/Betsy Crower t/` Edits the name of the 2nd person to be `Betsy Crower` and clears all existing tags.

Output:

![editPerson success](images/editPerson_success.png)

Errors:

- Incorrect parameters or command format
![editPerson error](images/error/editPerson_error.png)

- Incorrect or missing index
![editPerson wrongIndex](images/error/editPerson_wrongIndex.png)

<div style="page-break-after: always;"></div>


### 5. Find specific person: `findPerson`

Type in a few keywords linked to a person's name, and the matching persons' details will be displayed on screen.

{% include admonition.html type="note" title="Note" body="

This command hides all persons that do not match the search criteria. <br>
To reset the Persons view, simply run the <code>listPerson</code> command to list all persons.

" %}

Format:

```
findPerson KEYWORD [MORE_KEYWORDS]…
```

- At least one keyword is required to search.
- The search is case-insensitive. e.g `hans` will match `Hans`.
- The order of the keywords does not matter. e.g. `Hans Bo` will match `Bo Hans`.
- Only the name is searched.
- Only full words will be matched e.g. `Han` will not match `Hans`.
- Persons matching at least one keyword will be returned (i.e. `OR` search).
  - e.g. `Hans Bo` will match `Hans Gruber`, `Bo Yang`.

Examples:

- `findPerson John` returns `john` and `John Doe`
- `findPerson alex david` returns `Alex Yeoh`, `David Li`
- Assuming `Jane` is not in your contacts list, `findPerson Jane` returns `0 Persons Listed`.

Output:

- There are search outcomes to be displayed.

![findPerson success with a list](images/output/findPerson_success.png)


- There are no search outcomes to be displayed.

![findPerson success with zero results](images/output/findPerson_noResults.png)

Errors:

- Incorrect number of parameters.

![findPerson error](images/error/findPerson_error.png)

<div style="page-break-after: always;"></div>


### 6. Deleting a contact : `deletePerson`

{% include admonition.html type="danger" title="Potentially Dangerous Operation!" body="This action is irreversible." %}

Erase an outdated vendor from your contact list with ease.

Format:

```
deletePerson INDEX
```

- Deletes the person at the specified `INDEX`.
- The index refers to the index number shown in the displayed person list.
- The index **_must be a positive integer_** 1, 2, 3, …

Examples:

- `listPerson` followed by `deletePerson 2` deletes the 2nd person in the contact list.
- `findPerson Betsy` followed by `deletePerson 1` deletes the 1st person in the results of the `findPerson` command.

Output:

![deletePerson success](images/deletePerson_success.png)

Errors:

![deletePerson error](images/error/deletePerson_error.png)

<div style="page-break-after: always;"></div>


### 7. Clearing all entries : `deleteAllPerson`

{% include admonition.html type="danger" title="Potentially Dangerous Operation!" body="
AddressBook will discard all Person data and start with an empty data file at the next run.<br>" %}

Allows you to remove all entries from your contact list.

Format:

```
deleteAllPerson
```

Examples:

- `deleteAllPerson`

Output:

![deleteAllPerson success](images/deleteAllPerson_success.png)

<div style="page-break-after: always;"></div>


### 8. Adding a task: `addTask`

Adds a person, a vendor, to your contact list.

Format:

```
addTask n/NAME e/EVENT
```

{% include admonition.html type="info" title="A person can have any number of tags (including 0)." %}

Examples:

- `addTask n/Get Flowers e/Wedding Anniversary`
- `addTask n/Call Caterers e/Reunion Dinner`

Output:

![addTask_success](images/output/addTask_success.png)

Errors:

![addTask_error](images/error/addTask_error.png)

<div style="page-break-after: always;"></div>


### 9. Listing all tasks : `listTasks`

Provides you with a complete list of tasks in your task list.

Format:

```
listTasks
```

Examples:

- `listTask`

Output:

![listTask_success](images/output/listTask_success.png)


<div style="page-break-after: always;"></div>


### 11. Find specific task: `findTask`

You can locate tasks containing your specified keywords in their title and/or note.

{% include admonition.html type="note" title="Note" body="

This command hides all Tasks that do not match the search criteria. <br>
To reset the Tasks view, simply run the <code>listTasks</code> command to list all Tasks.

" %}

Format:

```
findTask KEYWORD [MORE_KEYWORDS]…
```

- At least one keyword is required to search.
- The search is case-insensitive. e.g `call` will match `Call`.
- The order of the keywords does not matter. e.g. `Call Caterer` will match `Caterer Call`.
- Both the title and note of a task is searched.
- Only full words will be matched e.g. `Call` will not match `Calls`.
- Tasks matching at least one keyword in either the title or the note will be returned (i.e. `OR` search).
  - e.g. `Call Wedding` will match `Call Hotel`, `Wedding Anniversary`.

Examples:

- `findTask Call Wedding` 
  - Finds tasks with titles or notes containing either `Call` or `Wedding`.
- `findTask Photography`
  - Finds tasks with titles or notes containing `Photography`.
- `findTask`
  - Negative example as no keywords are specified.

Output:
- Both tasks are displayed as Task 1 has the word `Call` in its title and Task 2 has the word `Wedding` in its note.

  ![findTask_success](images/output/findTask_success.png)

- There are no tasks to be displayed, as no Task has the word `Photography` in its title or note.
  
  ![findTask_noResults](images/output/findTask_noResults.png)

Errors:

- No keywords are specified.
  
  ![findTask_error](images/error/findTask_error.png)


<div style="page-break-after: always;"></div>


### 11. Deleting a task : `deleteTask`

{% include admonition.html type="danger" title="Potentially Dangerous Operation!" body="This action is irreversible." %}

You can remove the old vendor's specified contact from your contact list.

Format:

`deleteTask INDEX`

- Deletes the task at the specified `INDEX`.
- The index refers to the index number shown in the displayed task list.
- The index **_must be a positive integer_** 1, 2, 3, …

Examples:

- `listTask` followed by `deleteTask 2` deletes the 2nd task in the task list.
- `findTask Call` followed by `deleteTask 1` deletes the 1st task in the results of the `findTask` command.

Output:

![deleteTask_success](images/output/deleteTask_success.png)

Errors:

![deleteTask_error](images/error/deleteTask_error.png)


<div style="page-break-after: always;"></div>


### 12. Exiting the program : `exit`

You can exit the program.

Format: `exit`

<div style="page-break-after: always;"></div>

## FAQ

{% include admonition.html type="question" title="How do I transfer my data to another computer?" body="
Install the app in the other computer and overwrite the empty data file with your previous save file.

" %}

<div style="page-break-after: always;"></div>

## Known issues

{% include admonition.html type="bug" title="Bug: The GUI will open off-screen if you switch between multiple screens." body="

The remedy is to delete the <code>preferences.json</code> file created by the application before running the application again.
"%}
