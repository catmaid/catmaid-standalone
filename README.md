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
