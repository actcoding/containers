ARG PLANE_VERSION=0.22

FROM makeplane/plane-proxy:v${PLANE_VERSION}-dev

RUN sed -i 's|http://plane-minio:9000/uploads/|http://s3_storage:9000/${BUCKET_NAME}/|g' /etc/nginx/nginx.conf.template
