
### Context

Basically means current directory in the Dockerfile.
- du -shc * (current folder of Dockerfile)
- cd ..
- docker build -t image_name -f Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	.
	(Error)
- docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	.
	(Error)
- docker build -t image_name -f docker-images/Dockerfile3
	--build-arg user=ricardo --build-arg httpd_package=httpd
	docker-images/ 
	(Success)