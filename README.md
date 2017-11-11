Running
=======

### If you want to run everything in one line:

*Note: this will make a `/tmp/codalab` directory*

```
mkdir -p /tmp/codalab && docker run \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /tmp/codalab:/tmp/codalab \
    -d \
    --name compute_worker \
    --env BROKER_URL=<queue broker url> \
    --restart unless-stopped \
    ckcollab/competitions-v1-compute-worker:latest
```


### If you want to run using `.env` configuration:

Edit `.env_sample` and save it as `.env`

Make sure the temp directory you select is created and pass it in this command

```
docker run \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v <SUBMISSION TEMP DIRECTORY>:<SUBMISSION TEMP DIRECTORY> \
    -d \
    --name compute_worker \
    --env-file .env \
    --restart unless-stopped \
    ckcollab/competitions-v1-compute-worker:latest
```

### To get output of the worker

```
$ docker logs -f compute_worker
```

### To stop the worker

```
$ docker kill compute_worker
```


Development
===========

To re-build the image:

```
docker build -t competitions-v1-compute-worker .
```

Updating the image

```
docker build -t ckcollab/competitions-v1-compute-worker:latest .
docker push ckcollab/competitions-v1-compute-worker
```


Special env flags
=================


### DONT_FINALIZE_SUBMISSION

Sometimes it may be useful to pause the compute worker and return instead of finishing a submission. This leaves the
submission in a state where it hasn't been cleaned up yet and you can attempt to re-run it manually.

