# project-iiif-annotate

> A PyBossa project for generating Web Annotations from a IIIF-compatible image viewer.

Presents a zoomable image, retrieved via the
[IIIF Image API](http://iiif.io/api/image/), and asks volunteers to annotate
areas of that image (tag the titles, transcribe the names etc.). Contributions
are serialised according to the
[Web Annotations Data Model](https://www.w3.org/TR/annotation-model/).

Tasks are generated by choosing a task category from
[config/tasks.json](config/tasks.json) (e.g. `"title"`), a
IIIF manifest URI and an optional JSON file
containing the results data of a previous *project-iiif-annotate* project.

When a JSON results file is provided a task will be created for each result,
and each region now associated with that, the idea being that tasks can be
chained to highlight increasingly more specific regions in the text (e.g. all
names associated with a title). When a JSON results file is not used a task
will be created for each image.

## Configuring

By default, the project is configured for the LibCrowds playbills site. However,
alternative configurations can easily be created by making a copy of the
[config](config) folder, editing the files contained within and providing the
location of this custom config directory when running the scripts below.

See [config/README.md](config/README.md) for more details about the various configuration files.

## Build setup

Install [Node.js](https://nodejs.org/en/),
[Python](https://www.python.org/downloads/), and
[pbs](https://github.com/Scifabric/pbs), then:

```bash
# install
npm install

# clean
npm run clean

# generate project files
npm run generate -- <taskset> <manifest_uri> [--results=/path/to/results.json] [--config=/path/to/config]

# build the project
npm run build

# deploy
cd dist
pbs create_project
pbs add_tasks --tasks-file=tasks.json
pbs update_project
```

Once you have updated any additional settings on the server (category,
thumbnail, redundancy, webhooks etc.), the project is ready to be published.

## Developing

Build the project as above, then run the following:

```bash
# watch for JavaScript changes
npm run dev

# auto-update on the server
cd dist
pbs update_project --watch
```
