Helps build multi container system. 
```bash
docker-compose version
docker-compose
```
- need a .yml file.

```bash
mkdir docker-compose/
ll
#docker-compose.yml
docker run -d --name nginx -p 8080:80 nginx:alpine
```

```bash
docker-compose is defined by 4 options
- version:
- services:
- volumes: (optional)
- networks: (optional)
```

```bash
version: '3' #docker version
services: 
	web: #just a name as in --name in docker run
		image: nginx:alpine
		container_name: nginx_compose
		ports: 
			"9090:80"
```

```bash
vi docker-compose.yml
#paste the content
docker-compose up -d
docker ps
docker-compose down
```

### Environment Variables
