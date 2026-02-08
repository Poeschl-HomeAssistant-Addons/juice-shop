ARG BUILD_FROM
FROM $BUILD_FROM AS BUILD

ENV LANG C.UTF-8

RUN apk add --no-cache \
	git=2.49.1-r0 \
	npm=11.6.4-r0 \
	python3=3.12.12-r0 \
	build-base=0.5-r3

# Splited RUN for better layer caching
# hadolint ignore=DL3059
RUN git config --global advice.detachedHead false && \
    git clone https://github.com/juice-shop/juice-shop.git -b v19.0.0 /app

WORKDIR /app
RUN npm install typescript@5.9.2 ts-node@10.9.2 && \
	npm install --omit=dev --unsafe-perm && \
	npm dedupe && \
	rm -rf frontend/node_modules && \
	rm -rf frontend/.angular && \
	rm -rf frontend/src/assets && \
	mkdir logs && \
    chown -R 65532 logs && \
    chgrp -R 0 ftp/ frontend/dist/ logs/ data/ i18n/ && \
    chmod -R g=u ftp/ frontend/dist/ logs/ data/ i18n/


FROM $BUILD_FROM AS RUNNING

RUN apk add --no-cache \
    npm=11.6.4-r0

WORKDIR /app

COPY --from=BUILD /app /app

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
