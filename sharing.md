# Metadata and Reproducibility

For data science, "reproducibility" must practically include enough technical information to perform the same workflows on the same data, while also presenting that information in a way that is accessible to other scientists in the field.  Most tools either suffer from limitations in what provenance metadata they track or in the accessibility of leveraging that metadata and sharing it easily.  The Science APIs try to maintain as much relevant information as possible automatically and also lots of flexibility for capturing additional metadata.  In this section, we will look at the provenance metadata kept for data and jobs, how to share it with others, and how to associate free-form metadata with a specific file or job.

## Files and Data

The Science APIs automatically track the history of files that it knows about.  Try uploading a file, sharing it, and looking at the metadata that is automatically captured.

```
files-upload -v -F files/picksumipsum.txt -S filter.public.storage.system apiuser
```

You will see a progress bar while the file uploads, followed by a response from the server with a description of the uploaded file. Agave does not block during data movement operations, so it may be just a second before the file physically shows up on the remote system.

```
{
  "internalUsername": null,
  "lastModified": "2014-09-03T10:28:09.943-05:00",
  "name": "picksumipsum.txt",
  "nativeFormat": "raw",
  "owner": "apiuser",
  "path": "/iplant/home/apiuser/picksumipsum.txt",
  "source": "http://129.114.60.211/picksumipsum.txt",
  "status": "STAGING_QUEUED",
  "systemId": "filter.public.storage.system",
  "uuid": "0001409758089943-5056a550b8-0001-002",
  "_links": {
    "history": {
      "href": "https://api.example.com/files/v2/history/system/filter.public.storage.system/apiuser/picksumipsum.txt"
    },
    "self": {
      "href": "https://api.example.com/files/v2/media/system/filter.public.storage.system/apiuser/picksumipsum.txt"
    },
    "system": {
      "href": "https://api.example.com/systems/v2/filter.public.storage.system"
    }
  }
}
```

iOne of the key things to note is that Agave keeps track of file actions by associating a UUID with the file that was acted on. Next, share this file with someone else 

```
files-pems-update -U anotheruser -P READ -S filter.public.storage.system /iplant/home/apiuser/picksumipsum.txt
```

You can share this file with specific collaborators, and you can also independently grant read, write, and execute permissions for that collaborator.  This information is kept both in the permissions themselves, but also in the event history for that file.

```
files-pems-list filter.public.storage.system /iplant/home/apiuser/picksumipsum.txt

files-history -v -S filter.public.storage.system apiuser/picksumipsum.tx
```

Agave tracks every direct action it takes on a file or directory. Additionally, it will make note of any indirect action it observes about file or directory. Examples of direct action are transferring a directory from one system to another or renaming a file. Examples of indirect action are a user manually deleting a file from the command line.

Agave does not own the storage and execution systems you access through the REST APIs, so it cannot guarantee that it will be aware of everything that happens on that file system. Thus, Agave takes a best-effort approach to provenance allowing you to choose, through your own use of best practices, how thorough you want the provenance trail of your data to be.

## Jobs and Workflows


