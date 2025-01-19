ARG OS
FROM golang:${OS} AS build

ARG OS
ENV BASE=${OS}

WORKDIR /app
COPY src ./
RUN CGO_ENABLED=0 go build -o main

FROM scratch AS release
COPY --from=build /app/main /app/
WORKDIR /app
CMD ["/app/main"]