# goserver

Small Go HTTP server plus a separate Python text-report script used for container exercises.

This project is intentionally minimal and not meant to solve a real problem; it exists purely for educational purposes to learn Docker.

## What is here

- `main.go` - Go HTTP server with a single `/` route returning a small HTML page.
- `goserver` - prebuilt Go binary used by `Dockerfile`.
- `main.py` - Python script that prints a character frequency report for `books/frankenstein.txt`.
- `books/` - text input for the Python script.
- `Dockerfile` - minimal image that runs the Go binary.
- `Dockerfile.py` - build Python 3.10.8 from source and run the report script.

## Requirements

- Go 1.25.5+ for building/running the Go server locally.
- Python 3.x for running the report locally (Dockerfile builds 3.10.8 inside the image).
- Docker (optional) for container runs.

## Run the Go server locally

The server requires `PORT` to be set.

```bash
PORT=8991 go run main.go
```

Then open `http://localhost:8991/` in a browser or:

```bash
curl http://localhost:8991/
```

## Build the Go binary

`Dockerfile` expects a `goserver` binary in the repo root.

```bash
go build -o goserver .
```

## Run the Go server in Docker

```bash
go build -o goserver .
docker build -t goserver -f Dockerfile .
docker run --rm -p 8991:8991 -e PORT=8991 goserver
```

## Run the Python report locally

```bash
python3 main.py
```

Example output starts with:

```
--- Begin report of books/frankenstein.txt ---
```

## Run the Python report in Docker

`Dockerfile.py` builds Python from source, so the build can take a while.

```bash
docker build -t bookbot -f Dockerfile.py .
docker run --rm bookbot
```

## Notes

- The Go server listens on `":" + PORT`; if `PORT` is empty, `ListenAndServe` will fail.
- The Python script only reads `books/frankenstein.txt`; update `book_path` in `main.py` to use other files.
