# Session 09 - Exercise 4: Simple data persistence with a Docker Volume

## Goal

Create a named volume and show that data written to it survives after the container that wrote it is gone.

## Commands

### 1. Create the volume

```bash
docker volume create easy-volume
```

Check that it exists:

```bash
docker volume ls
```

### 2. Write data from a first Alpine container

```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```

- `-v easy-volume:/data` mounts the named volume at `/data` inside the container.
- `--rm` deletes the container as soon as the command finishes, so after this step the writing container no longer exists.

### 3. Read the data back from a second Alpine container

```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```

## Expected result

```
Luu tru du lieu Docker
```

## Explanation

The two containers are completely separate, and the first one is removed before the second one starts. The file is still there because it was not stored in the container's own writable layer, but in the volume `easy-volume`, which Docker manages on the host independently of any container's lifecycle. A volume is only deleted when it is removed explicitly.

## Useful checks

```bash
docker volume inspect easy-volume   # see the mountpoint on the host
docker ps -a                        # confirm no leftover containers (because of --rm)
```

## Cleanup

```bash
docker volume rm easy-volume
```
