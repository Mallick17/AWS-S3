# AWS S3 CLI
### S3 CLI Sync Commands
- 1st Create a folder inside a bucket in s3 and
- 2nd Create a folder and add some files in local directory
- 3rd Copied 2 files `dump.py , user.env` files from the local folder to s3 bucket folder

<img width="1204" height="321" alt="image" src="https://github.com/user-attachments/assets/04bb6965-ea7a-4352-af84-58c9e36eb437" />

- 4th modified the `dump.py` file and didnt change the `uesr.env` file and gonna try the sync command and there are few other files in the local folder which isnt synced.
```shell
pwd
/Users/gyanaranjan.mallick/Downloads/testing/test

aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
upload: ./dump.py to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/dump.py
upload: ./dump1.py to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/dump1.py
upload: ./user-laravel.env to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/user-laravel.env
```
<img width="1189" height="392" alt="image" src="https://github.com/user-attachments/assets/f15b490c-ba8f-49e2-8caa-6c8bac014251" />

- `aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/` it overwrites the modified files and adds the misisng files which is not present in s3 bucket after comparing the local folder.

Absolutely! Let’s clarify **why `aws s3 sync` cannot avoid overwriting modified files**, and then integrate that explanation into a clean, cohesive documentation.

---

## Why `aws s3 sync` Can’t Avoid Overwriting Modified Local Files

* **Designed as a one-way sync**, `aws s3 sync` prioritizes the source (local) version. If a local file is newer or different, it *always* overwrites the existing object in S3 ([Stack Overflow][1]).

* It determines whether to upload primarily based on three criteria:

  1. The local file doesn’t exist in S3.
  2. The file size differs.
  3. The local file is newer than the S3 version (based on timestamps) ([Zenduty Community][2], [awsfundamentals.com][3]).

* **There is no option** in `aws s3 sync` to say “skip overwriting if the file exists, even if it’s different.” The CLI offers no flag like `--ignore-existing` or `--no-overwrite` for `sync` ([Stack Overflow][1]).

In short—**if `sync` sees a difference, it will overwrite**, with *no built-in method to change that behavior*.


<Details>
  <summary>Click to view the clean documentation in sync</summary>

## Complete Documentation

### Scenario Steps

1. **Create a folder inside the S3 bucket**

   ```bash
   aws s3api put-object --bucket elasticbeanstalk-ap-south-1-508351649560 --key sync-commands-test/
   ```

2. **Create a local folder and add files**

   ```bash
   mkdir -p ~/Downloads/testing/test
   cd ~/Downloads/testing/test

   echo "print('hello world')" > dump.py
   echo "APP_ENV=local" > user.env
   echo "print('second file')" > dump1.py
   echo "APP_ENV=laravel" > user-laravel.env
   ```

3. **Copy two files to S3 using `cp`**

   ```bash
   aws s3 cp dump.py s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   aws s3 cp user.env s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   ```

4. **Modify one file locally**

   ```bash
   echo "print('modified content')" > dump.py
   ```

5. **Run `sync`**

   ```bash
   aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   ```

   Output:

   ```
   upload: ./dump.py to s3://…/dump.py
   upload: ./dump1.py to s3://…/dump1.py
   upload: ./user-laravel.env to s3://…/user-laravel.env
   ```

6. **What Happened**

   * `dump.py`: Overwritten in S3 (because it's changed locally).
   * `user.env`: Left untouched (unchanged locally).
   * `dump1.py` & `user-laravel.env`: Newly uploaded (missing in S3).

---

### Feature Comparison Table

| Desired Behavior                   | `aws s3 sync` Result   | Alternative (`aws s3 cp --ignore-existing`) |
| ---------------------------------- | ---------------------- | ------------------------------------------- |
| Upload missing files only          | Yes                    | Yes                                         |
| Skip existing but modified files   | No (overwrites)        | Yes (skips)                                 |
| Full sync (missing + changed)      | Yes (default behavior) | No (doesn't overwrite)                      |
| Built-in flag to prevent overwrite | No                     | Yes (`--ignore-existing`)                   |

---

### Why `sync` Overwrites Modified Local Files

* `aws s3 sync` is built to ensure the destination mirrors the source; if a local file is different (by size or timestamp), it assumes the local copy is authoritative and deliberately overwrites ([Zenduty Community][2], [awsfundamentals.com][3]).

* The CLI offers no switch to reverse that logic (e.g., “never overwrite”), meaning **you cannot use `sync` if you want to only upload missing files and leave modified ones untouched** ([Stack Overflow][1]).

---

### Final Recommendation

Use `aws s3 sync` when you want the bucket to fully reflect your local folder—missing files are uploaded, changed files are updated.

But if your goal is *only to upload missing files and leave any modified local files alone*, you should use:

```bash
aws s3 cp . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/ --recursive --ignore-existing
```

---

</Details>

---
