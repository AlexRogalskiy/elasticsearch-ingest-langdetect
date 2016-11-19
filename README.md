# Elasticsearch Langdetect Ingest Processor

Uses the [langdetect](https://github.com/YouCruit/language-detection/) plugin to try to find out the language used in a field.

## Usage


```
PUT _ingest/pipeline/langdetect-pipeline
{
  "description": "A pipeline to do whatever",
  "processors": [
    {
      "langdetect" : {
        "field" : "my_field"
        "target_field" : "language"
      }
    }
  ]
}

PUT /my-index/my-type/1?pipeline_id=langdetect-pipeline
{
  "my_field" : "This is hopefully an english text, that will be detected."
}

GET /my-index/my-type/1
{
  "my_field" : "This is hopefully an english text, that will be detected.",
  "language": "en"
}
```

## Configuration

| Parameter | Use |
| --- | --- |
| field         | Field name of where to read the content from |
| target_field  | Field name to write the language to |
| max_length    | Max length of of characters to read, defaults to 10kb, requires a byte size value, like 1mb |

## Setup

In order to install this plugin, you need to create a zip distribution first by running

```bash
gradle clean check
```

This will produce a zip file in `build/distributions`.

After building the zip file, you can install it like this

```bash
bin/plugin install file:///path/to/ingest-langdetect/build/distribution/ingest-langdetect-0.0.1-SNAPSHOT.zip
```

## Side notes

In order to cope with the security manager, a special factory is used to load the languages from the classpath.
You can check out the `SecureDetectorFactory` class. This implementation also does not use jsonic to prevent the use of reflection when loading the languages.