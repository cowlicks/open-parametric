# Problem

Websites that share models for 3D printing, like printables.com have a problem.
The just provide a way for you to download static user submitted static files.
Many models are not one-size-fits all and need to be adjusted, or resized, often based on some paraameters.
For example, for a screw, you might want to tweak its pitch, length, thread count, etc.
There is currently no solution for doing this in browser, in a nice way.
Instead, users submit openscad, or fusion 360 files (or others) that must be opened in their own respective program to tweak the parameters.

# Concept

* static website, parametrization happens entirely on the front-end
* run user submitted parametrization code SAFELY with webassembly
* open source and standardized to encourage adoption
* parameter input UI from automatically generated from json schema `parameters.json`
* create parameterized models via existing tools like blender, openscad
* batteries included library 3D model websites, to encourage adoption


# The format

A model meeting this standard must provide 2 things: 
* A wasm binary that take parameters and outputs an STL file.
* A schema describing the parameters the binary takes.

## The Schema

The file should be called `parameters.json`.
It contains some top level metadata, and a list of parameters.

Metadata
* name
* output type: STL
* ???


Each parameter has at least:
```json
{
  "name": <parameter name>,
  "input_type": {
    "type": number|bool|string|enum,
    ...type specific data 
  "default_value": 42,
}
```

We provide:
* a validation library for verifying user submitted schema
* a library for automatically generating a UI from the  `parameters.json`

## The binary

The wasm binary should have just one function with the signature: `(parameters) -> bytes`.
Where the `parameters` are those from the `parameters.json` file. The `byts` are the raw for the 3D model (probably in STL format)
