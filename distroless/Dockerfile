FROM python:3-slim AS env
COPY . /app
WORKDIR /app

FROM gcr.io/distroless/python3
COPY --from=env /app /app
WORKDIR /app
CMD ["hello.py"]
