Use case: file/media upload (and tracking with hasura

- Obviously, we want to use a filestorage facility (S3, Firebase Storage, etc etc) for it, so the problem is how to integrate with hasura

- 1) Dmytro's approach is use a hasura action (external integration) and transport the file content in base64 format, then do the integration part as part of the action handler

  - good points: perfect in terms of abstracting the functionality, client is independent from the storage solution, completely integrated with hasura auth system (permissions, etc etc) and goodies (multiple inserts in a single mutation, transactions, etc)

  - bad points: overhead (30% extra in file uploads from base64 conversion), prolly won't work for big files (>10Mb?)

- 2) External handler: a custom endpoint pipes multipart file uploads into the final storage after doing some checks with hasura... used by nhost and extracted as utility in this github: toastedtoast/hasura-storage

  - good points: not bad performance, should escalate well

  - bad points: external to hasura, all checks must be done "by hand", client should use

- 3) My current thought: use presigned urls with deriving actions: let's say we have a File table, we create an action for uploadingFileRequest which generates and returns a presigned url and insert a row in the file table... then, client can use it to upload files straight to the file storage (ex. S3... best performance)... after success, client only needs to mutate File table with url

  - good points: best escalability, with some client wrapping should be quite smooth

  - bad points: keeps being externally to hasura and involves client side... no transactions, etc etc
