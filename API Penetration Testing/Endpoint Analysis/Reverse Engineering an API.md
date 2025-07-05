Two ways to reverse engineer an API:
1. Using postman to manually collect API requests and building it's own collection.
2. Automatically build one using mitmproxy2swagger

- Why do you need to learn to reverse engineer an API?
  - in case there is no documentations.

automating reversing API docs using mitmproxy2swagger
```
sudo mitmproxy2swagger -i /Downloads/flows -o spec.yml -p http://crapi.apisec.ai -f flow
```

