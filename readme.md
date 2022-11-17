When you have created this file, head back to your terminal and make it executable with

```bash
chmod +x entrypoint.sh
```

Copy

What will this script do? It will change the UID of the `www-data` user in the container to the value of the host user (our local user) by picking up the environment variable that we pass to the container upon start-up.

Passing environment variables from the host to the container can also be done in various ways, here we will utilize a file which contains the variable and its value. To accomplish this, we can use the same commands as above to find out the value for our local user, but this time we will create a file which contains this value as environment variable in `var=value` format:

In your terminal, `cd` into the `docker-example-3` folder and type the following command:

```bash
echo -e "USERID=$(id -u)" > id.env
```

Copy

This command will get the ID with the `id -u` command we already used above, and output it into a file. The resulting `id.env` file will look similar to this, only with your UID, which might differ from mine.

```
USERID=1001
```

At this point, your folder structure should look like this:

- docker-example-3starterkitdefault.confDockerfileentrypoint.shid.env

After stopping and removing the previous container we build a new image, start a new container and see what happens:

```bash
docker build -t docker-starterkit .
docker run -d --name mycontainer -p 80:80 \
  --mount type=bind,source=$(pwd)/starterkit,destination=/var/www/html \
  --env-file id.env docker-starterkit
```

Copy

At this point, you can start making changes locally, view them in the browser and they will of course still be there in your filesystem even after you remove the container again.