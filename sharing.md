# Metadata and Reproducibility

For data science, "reproducibility" must practically include enough technical information to perform the same workflows on the same data, while also presenting that information in a way that is accessible to other scientists in the field.  Most tools either suffer from limitations in what provenance metadata they track or in the accessibility of leveraging that metadata and sharing it easily.  The Science APIs try to maintain as much relevant information as possible automatically and also lots of flexibility for capturing additional metadata.  In this section, we will look at the provenance metadata kept for data and jobs, how to share it with others, and how to associate free-form metadata with a specific file or job.

## Files and Data

The Science APIs automatically track the history of files that it knows about.  We uploaded and manipulated some files during the "Systems and Data Movement" section.  Now we can try sharing those files and looking at the metadata that is automatically captured.

One of the key things to note is that Agave keeps track of file actions that was acted on every file you upload through the platform or use in a job. Let's take a look at a file we uploaded earlier.

```
files-history -S akes2016-storage-USERNAME ./system.md
```

Let's share a file with our neighbor and see what is recorded. If we want to share a file that is on our own private system, we must first give the other user access on our private system.  If we share a file from a public system like data.iplantcollaborative.org, we can share the file directly like the following:

```
files-pems-update -U anotheruser -P READ -S data.iplantcollaborative.org CYVERSE-USERNAME/systems.md
```

You can share this file with specific collaborators, and you can also independently grant read, write, and execute permissions for that collaborator.  This information is kept both in the permissions themselves, but also in the event history for that file.

```
files-pems-list CYVERSE-USERNAME/systems.md

files-history -v CYVERSE_USERNAME/systems.md
```

Agave tracks every direct action it takes on a file or directory. Additionally, it will make note of any indirect action it observes about file or directory. Examples of direct action are transferring a directory from one system to another or renaming a file. Examples of indirect action are a user manually deleting a file from the command line.

Agave does not own the storage and execution systems you access through the REST APIs, so it cannot guarantee that it will be aware of everything that happens on that file system. Thus, Agave takes a best-effort approach to provenance allowing you to choose, through your own use of best practices, how thorough you want the provenance trail of your data to be.

## Jobs

Earlier in the day, you should have run an app.  To see the jobs you have run, use this command:

```
jobs-list
```

See it in the list?  By default, that command just outputs the ID of the job and the status.  For a little more information on a job, use the -v argument.  You can specify the job ID too.  Try this:

```
jobs-list -v 1085811849368825370-242ac114-0001-007
```

The output is in Javascript Object Notation (JSON), but it contains a lot of information, including when the job started and finished, what the parameters are for the job, and where it archived the output.  For even more information about the job while it was running, try this:

```
jobs-history 1085811849368825370-242ac114-0001-007
```

## Apps

Beyond sharing data and jobs, you can share the underlying apps so that others can independently run jobs on the app you created.  "Read" permission on an app will let other just see your app wrapper and how you setup the inputs and outputs.  They could then "clone" that app to a system they have access to if they wanted.  "Execute" permissions will let them run the app, but only if you also give them access to the system (it if isn't a public system) just like sharing files.  Keep in mind that apps are tied to specific systems, and systems are tied to authentication credentials.  That means that when someone else runs your app, they are running it on your system with the credentials you established.  If you made an app this morning, try sharing it with your neighbor.

```
apps-pems-update -u anotheruser -p READ app-name-you-own
```

## Metadata

Though we will not do a hands on portion for the "metadata" service.  Agave supports storing arbitrary metadata that can have "Association UUIDs" that connect the metadata with any other object in Agave (e.g. jobs, apps, data, systems, etc.).  The metadata is stored in a Mongo database and exposed through API endpoints. Some sites like www.vdjserver.org make heavy use of that service behind the scenes.  It is very powerful, but not suited to a brief hands-on section.

