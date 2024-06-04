FROM makeplane/plane-proxy:stable

RUN sed 's|http://plane-minio:9000/uploads/|http://s3_storage:9000/${BUCKET_NAME}|g' /etc/nginx/nginx.conf.template
