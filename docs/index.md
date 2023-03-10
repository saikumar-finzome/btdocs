# Backtify


## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

### Dockerfile

Now in the same project directory create a file `Dockerfile` with:

```{ .dockerfile .annotate }
# (1)
FROM python:3.9

# (2)
WORKDIR /code

# (3)
COPY ./requirements.txt /code/requirements.txt

# (4)
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt

# (5)
COPY ./app /code/app

# (6)
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```

1. Start from the official Python base image.

2. Set the current working directory to `/code`.

    This is where we'll put the `requirements.txt` file and the `app` directory.

3. Copy the file with the requirements to the `/code` directory.

    Copy **only** the file with the requirements first, not the rest of the code.

    As this file **doesn't change often**, Docker will detect it and use the **cache** for this step, enabling the cache for the next step too.

4. Install the package dependencies in the requirements file.

    The `--no-cache-dir` option tells `pip` to not save the downloaded packages locally, as that is only if `pip` was going to be run again to install the same packages, but that's not the case when working with containers.

    !!! note
        The `--no-cache-dir` is only related to `pip`, it has nothing to do with Docker or containers.

    The `--upgrade` option tells `pip` to upgrade the packages if they are already installed.

    Because the previous step copying the file could be detected by the **Docker cache**, this step will also **use the Docker cache** when available.

    Using the cache in this step will **save** you a lot of **time** when building the image again and again during development, instead of **downloading and installing** all the dependencies **every time**.

5. Copy the `./app` directory inside the `/code` directory.

    As this has all the code which is what **changes most frequently** the Docker **cache** won't be used for this or any **following steps** easily.

    So, it's important to put this **near the end** of the `Dockerfile`, to optimize the container image build times.

6. Set the **command** to run the `uvicorn` server.

    `CMD` takes a list of strings, each of these strings is what you would type in the command line separated by spaces.

    This command will be run from the **current working directory**, the same `/code` directory you set above with `WORKDIR /code`.

    Because the program will be started at `/code` and inside of it is the directory `./app` with your code, **Uvicorn** will be able to see and **import** `app` from `app.main`.

!!! tip
    Review what each line does by clicking each number bubble in the code. 👆

You should now have a directory structure like:

```
.
├── app
│   ├── __init__.py
│   └── main.py
├── Dockerfile
└── requirements.txt
```