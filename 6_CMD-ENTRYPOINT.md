# CMD vs ENTRYPOINT

Both `CMD` and `ENTRYPOINT` define what command runs inside a container when it starts, but they behave differently in terms of overriding and flexibility.

## 1. CMD (Command)

- Sets a **default command** for the container.

- Can be **overwritten** at runtime when passing a command in `docker run`.

- It is best for **providing default arguments** that can be changed easily.

### Syntax

CMD has two formats:

1. **Shell form** (executes in a shell)
   ```dockerfile
   CMD python app.py
   ```

2. **Exec form** (recommended, does not invoke a shell)
   ```dockerfile
   CMD ["python", "app.py"]
   ```

### Example

**Dockerfile:**
```dockerfile
FROM python:3.9
COPY app.py /app.py
CMD ["python", "app.py"]
```

**Run:**
```sh
docker run my-container
```
To override `CMD`:
```sh
docker run my-container python another_script.py
```
Here, `another_script.py` replaces `app.py`.

## 2. ENTRYPOINT

- Sets the **main command** that always runs in the container.

- Any command-line arguments passed in `docker run` are treated as arguments to `ENTRYPOINT`.

- It is best for **creating dedicated executables** (e.g., a service that always runs).

### Syntax

ENTRYPOINT has two formats:

1. **Shell form** (executes in a shell)
   ```dockerfile
   ENTRYPOINT python app.py
   ```

2. **Exec form** (recommended, does not invoke a shell)
   ```dockerfile
   ENTRYPOINT ["python", "app.py"]
   ```

### Example

**Dockerfile:**
```dockerfile
FROM python:3.9
COPY app.py /app.py
ENTRYPOINT ["python", "app.py"]
```

**Run:**
```sh
docker run my-container
```
To override `ENTRYPOINT`:
```sh
docker run --entrypoint python my-container another_script.py
```
This is required because `ENTRYPOINT` forces `python app.py` to always run.

## CMD + ENTRYPOINT Together

You can use `ENTRYPOINT` as the fixed command and `CMD` as default arguments.

**Example:**
```dockerfile
FROM python:3.9
COPY app.py /app.py
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Now:
```sh
docker run my-container
```
Runs:
```sh
python app.py
```
But if you run:
```sh
docker run my-container another_script.py
```
It runs:
```sh
python another_script.py
```

## When to Use What?

- **Use `CMD`** when providing a **default command** but allowing users to override it.

- **Use `ENTRYPOINT`** when defining a **mandatory executable** that should always run.

- **Use both (`ENTRYPOINT` + `CMD`)** when you need a default argument but want to allow changes.

