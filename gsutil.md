# `gsutil` (Google Storage util) cheatsheet

## Installation
* Last updated: September 4, 2020
* Source: [Google Cloud Documentation](https://cloud.google.com/sdk/docs#deb)

```bash
# 1. Add Cloud SDK URI as a package source.

echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

# 2. Install the necessary dependencies.
sudo apt-get install apt-transport-https ca-certificates gnupg

# 3. Import the Google Cloud public key.
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

# 4. Fetch the latest repository information, install Cloud SDK.
sudo apt-get update && sudo apt-get install google-cloud-sdk

# 5. Initialize Cloud SDK.
gcloud init
```

## Usage
* View the help menu and learn best practices.

```
gsutil help
```

* List existing buckets in the current project.

```
gsutil list
```

* Make a new bucket.

```
gsutil mb -l <location> gs://<bucket name>
```

* Copy (upload) local data to a Google Cloud Storage bucket.
* Download data from Google Cloud Storage to a local file.

```
# for single-file uploads
gsutil cp <local file> gs://<bucket name>

# for multi-file uploads
gsutil cp -R <local file> gs://<bucket name>

# download data
gsutil cp gs://<bucket name>/<blob> <local file> 

# for large data, provide the -m flag for multiprocessing and multithreading
gsutil cp -m <local file> gs://<bucket name>
```

* Show objects in existing bucket.

```
gsutil ls [-l] gs://<bucket name>
```

* Reconstruct object after uploading to Google Cloud Storage.

```
gsutil compose gs://<bucket name>/<blob glob> gs://<bucket name>/<blob>
```

* Delete object from Google Cloud Storage.

```
gsutil rm gs://<bucket name>/<blob>
```
