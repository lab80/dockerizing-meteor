# Seoul Meteor Meetup presentation: "Dockerizing Meteor"

Simple introduction to dockerizing meteor.

##Simple Todos example

After installing `boot2docker` and cloning `https://github.com/jaigouk/simple-todos`, we
run the following at the command-line:

```sh

boot2docker start

docker build -t="inlight/mongodb-replica-set" github.com/inlight-media/docker-mongodb-replica-set

docker run -i -t -d -p 27017:27017 -p 27018:27018 -p 27019:27019 --name mongodb inlight/mongodb-replica-set
docker exec -it mongodb bash
mongo
> rs.initiate()
>  rs.add('6e8d183167b4:27018') # use token in 'me'
>  rs.add('6e8d183167b4:27019')
>  rs.status()
> exit

MONGODB="mongodb://"$(boot2docker ip)":27017,"$(boot2docker ip)":27018,"$(boot2docker ip)":27019/?replicaSet=dbReplicaSet&connectTimeoutMS=300000"

docker build -t 'todos' .

docker run --name todos \
            -p 5000:5000 \
            --memory="128m" \
            -e MONGO_URL="$MONGODB" \
            -e ROOT_URL="http://127.0.0.1" \
            todos

```