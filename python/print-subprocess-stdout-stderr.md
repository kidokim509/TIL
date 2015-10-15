# Print stdout and stderr of subprocess

__NOTE__: Information taken from [Stack Overflow](https://www.google.co.kr/search?q=%23+Helper+function+to+add+the+O_NONBLOCK+flag+to+a+file+descriptor&oq=%23+Helper+function+to+add+the+O_NONBLOCK+flag+to+a+file+descriptor&aqs=chrome..69i57.197j0j1&sourceid=chrome&es_sm=91&ie=UTF-8)

Helper function to add the `O_NONBLOCK` flag to a file descriptor
```python
def make_async(fd):
    fcntl.fcntl(fd, fcntl.F_SETFL, fcntl.fcntl(fd, fcntl.F_GETFL) | os.O_NONBLOCK)
```

Helper function to read some data from a file descriptor, ignoring `EAGAIN` errors
```python
def read_async(fd):
    try:
        return fd.read()
    except IOError, e:
        if e.errno != errno.EAGAIN:
            raise e
        else:
            return ''
```

Helper function to execute shell command
(It returns `False` when stderr string has a word, `"ERROR"`.)
```python
def execute(command):
    process = Popen(command, stdout=PIPE, stderr=PIPE)
    make_async(process.stdout)
    make_async(process.stderr)

    stdout = str()
    stderr = str()
    returnCode = None

    while True:
        # Wait for data to become available
        select.select([process.stdout, process.stderr], [], [])

        # Try reading some data from each
        stdoutPiece = read_async(process.stdout)
        stderrPiece = read_async(process.stderr)

        if stdoutPiece:
            print stdoutPiece,
        if stderrPiece:
            print stderrPiece,

        stdout += stdoutPiece
        stderr += stderrPiece
        returnCode = process.poll()

        if returnCode != None:
            break;
            
    res = True
    try:
        if (stderr) and (stderr.index("ERROR")):
            res = False
        except ValueError:
            res = True

    return res
```
