# Welcome to [Slidev](https://github.com/slidevjs/slidev)!

To start the slide show:

- `pnpm install`
- `pnpm run dev`
- visit <http://localhost:3030>

Edit the [slides.md](./slides.md) to see the changes.

Learn more about Slidev at the [documentation](https://sli.dev/).

## First logs in splunk


```bash
$ curl --insecure -X POST https://localhost:8088/services/collector/event -H "Authorization: Splunk $TOKEN$" -H "Content-Type: application/json" -d '{"message": "hello-world", "level": "INFO"}'
{"text":"No data","code":5}

$ curl --insecure -X POST https://localhost:8088/services/collector/event \/services/collector/event \
  -H "Authorization: Splunk $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"event": {"message": "hello-world", "level": "INFO"}}'
{"text":"Success","code":0}
```

A proper token needs to be created in splunk HEC config before using it