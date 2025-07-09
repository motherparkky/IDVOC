FROM reg.undercloud.cri.epita.fr/docker/golang:1.21.5-alpine AS builder

LABEL maintainer="Sunwoo Park <sunwoo.park@epita.fr>"

WORKDIR /app

RUN apk --no-cache add curl bash

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN go build -o CommentsInteractor .

FROM reg.undercloud.cri.epita.fr/docker/golang:1.21.5-alpine

LABEL maintainer="Sunwoo Park <sunwoo.park@epita.fr>"

RUN apk --no-cache add curl bash && \
    adduser -D -u 1000 appuser

COPY --from=builder /app/CommentsInteractor /app/CommentsInteractor

ENV HOST=0.0.0.0
ENV PORT=9000
ENV REDIS_ENDPOINT=redis:6379

EXPOSE 9000

WORKDIR /app
USER appuser

CMD ["./CommentsInteractor"]