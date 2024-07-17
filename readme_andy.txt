//create  nodeapp 
docker build . -t nodeapp
//create network
docker network create backendnet

//start nodeapps
for i in {1..10}
do
  docker run --rm --hostname nodeapp${i} --name nodeapp${i} --network backendnet -d nodeapp
done
//stop nodeapps
for i in {1..10}
do
  docker kill nodeapp${i} 
done

// start ng1
docker run --rm --hostname ng1 --name nginx1 --network backendnet -p 80:8080 -v ./nginx1.conf:/etc/nginx/nginx.conf -d nginx
docker run --rm --hostname ng2 --name nginx2 --network backendnet -p 81:8080 -v ./nginx2.conf:/etc/nginx/nginx.conf -d nginx

//remove network
docker network rm backendnet