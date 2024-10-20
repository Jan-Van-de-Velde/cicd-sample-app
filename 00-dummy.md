# Lab Report: Prime Numbers in Python

## Student information

- Student name: Jan Van de Velde
- Student code: 202291382

## Assignment description

Learn to work with github actions and effeciently use dependabot to keep packages up to date in order to get an understanding of continues integration.

## Proof of work done

### build.yml for basic github actions

I have added this to yml to the github workflow folder [build.yml](./.github/workflows/build.yml).

```
name: Build and test
on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: List directory structure
        run: ls -R
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "20.9.0"
      - name: Install dependencies
        run: yarn install
        working-directory: webapp
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v2
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_ACCESS_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          context: ./webapp
          file: ./webapp/dockerfile
          tags: leduude/webapp:latest


```

I have added this to yml to the github folder [dependabot.yml](./.github/dependabot.yml).

```
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "."
    schedule:
      interval: "weekly"
  - package-ecosystem: "yarn"
    directory: "."
    schedule:
      interval: "weekly"
  - package-ecosystem: "npm"
    directory: "."
    schedule:
      interval: "weekly"


```

### Conclusion:

once you get the hang of github actions they can be a usefull tool to cut down significantly on development time however the initial setup can be tricky

## Reflection

### What was difficult?

- yarn linting left me with funky errors where it seems like i am using an incorrect version of node and that it no longer supports the --ext option even after updating the order of which build.yml runs its instructions it still seems to fail.
- dependabot setup was supposed to be easy however i left the yml in the workflow folder where it can not be interpretet properly so it kept on failing with the error message "nothing to do on the on case". which in hindsight should have made it obvious it shouldnt have been in there.

### What was easy?

- the initial setup from was rather easy untill linting.
- dependabot gave me some trouble initially however after placing it in the correct folder it was easy.(i seem to have missed that part in the documentation)

### What did I learn?

- how to correctly set up several github actions(i used several different build.yml files in order to test more effeciently)
- how to correctly set up dependabot.
- how to work with github secrets

### What would I do differently?

- with dependabot i would spend more time reading the documentation since it would have saved me a lot of time.

## Resources

- [Sieve of Eratosthenes - Wikipedia](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
- [Python `time` module documentation](https://docs.python.org/3/library/time.html)
- [Prime number theory and algorithms](https://www.geeksforgeeks.org/prime-numbers/)
