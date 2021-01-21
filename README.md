# reproducible-ml


### 01. Keep / generate data or binary files in a specific folder `storage/`

### 02. Setup GIT and DVC

- **GIT:** Initialize, add, commit (w/ data inside repo)

    ```
    $ git init
    $ git add .
    $ git commit -m"initial commit w/ data"
    ```

- **DVC:** Initialize, add data to a source (w/ data inside repo)

    ```
    $ dvc init
    $ dvc remote add -d localremote /tmp/dvc-storage
    $ dvc config core.analytics false
    ```

### 03. Delete contents of `storage/`. And add the removed folder names inside in `.gitignore` and place it inside `storage/`

`storage/.gitignore.io`
```
folder1/
folder2/
folder3/
```

### 04. **DVC run** create `.dvc` file(s) and ad code/data dependencies while running script 

- `-f` path and name to `.dvc` to be generated
- `-d` path to the file to be run using `dvc run`
- `-d` path and name to module.py dependency
- `-d` path to folder with data dependance
- `-o` path to output folder of this `dvc` run
- `-M` path and name to text based eg. json output on this `dvc` run
- finally command to run script

**Usage:**

    ```
    $ dvc run -f storage/folder-to-be-created.dvc \
    > \
    > -d path-to-python-file-being-run.py \
    > -d path-to-other-python-file-dep.py \
    > -d storage/some-data-folder \
    > \
    > -o storage/name-of-folder-being-edited-or-created \
    > -M storage/some-text-or-yml-or-json-file \
    > \
    > python path-to-python-file-being-run.py
    ```

**Example:**

    ```
    $ dvc run -f storage/models.py \
    > -d proj/train_model.py \
    > -d storage/fetures_data \
    > -o storage/models \
    > -M storage/train_history \
    > python proj/train_model.py
    ```

> **Note:**
>
> Git is tracking `.dvc` files associated with each folder/file(s) inside in storage/assets folder instead of actual folders that exist there. 
>
> Actual folders are ignored using `.gitignore` file.

### 05. **GIT:** `add` and `commit` the created `.dvc` files 

### 06. **GIT:** `tag` current experiment

```
$ git tag -a 'log-reg-experiment-0' -m 'Logistic regression w/o any hyperparams'
```

- **list taged experiments using**

    ```
    $ dvc metrics show -T
    ```

    Note: `metrics.dvc` must be created beforehand.

### 07. Make changes to code and for other experiment and run `dvc repro`