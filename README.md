# catmaid-standalone

A regular Docker image that uses the CATMAID base image and sets up PostgreSQL
and Nginx.

A more detailed explanation can be found in the [CATMAID
documentation](http://catmaid.readthedocs.io/en/stable/docker.html). To run it,
install docker and call

```
docker run -p 8080:80 --name catmaid catmaid/catmaid-standalone
```

Navigate your browser to [http://localhost:8080](http://localhost:8080)
and you should see the CATMAID landing page. You can log in as a superuser
with username "admin" and password "admin". The Docker image contains a few
example CATMAID projects and stacks, but you can add your own through the admin
page.

*Warning:* If this Docker container is network accessible, be sure to change the
default password of the admin user.

*Warning:* Any users, projects, stacks or annotations you add to the running
Docker container will by default be lost when you next run it. To save these
changes, you must [commit them with docker](https://docs.docker.com/engine/reference/commandline/commit/).
However, this is not a best practice for using Docker, and we currently do not
recommend this CATMAID Docker image for production use.

## Backup and restore

To backup the current database state of the container, the following can be
used:

```
docker exec -u postgres catmaid /usr/bin/pg_dumpall --clean -U postgres > backup.pgsql
```

To restore such a backup for a particular container, the Postgres backup needs
to be piped into the docker command:

```
cat backup.pgsql | docker run -p 8000:80 -i -e DB_FIXTURE=true --name catmaid catmaid/catmaid-standalone
```

## Update

Since by default all data is lost when the container is stopped, you might want
to backup your data (see above) and restore after the update.

Before updating the images, make sure to stop the containers using `docker stop
catmaid` (if you didn't used `--name` with `docker run`, use the container
ID instead of "catmaid").

First update the CATMAID base image:

```
docker pull catmaid/catmaid
```

Then, to update `catmaid-standalone` (regular Docker) use:

```
docker pull catmaid/catmaid-standalone
```

If no previous state should be persisted, the docker contaienr can be started
normally again:

```
docker run -p 8000:80 --name catmaid catmaid/catmaid-standalone
```

If you instead want to restore from a previously taken backup, see the "Backup
and restore" section above for how to run the image.
