# Kroki CI/CD pipeline

<a href="https://dash.elest.io/deploy?source=cicd&social=dockerCompose&url=https://github.com/elestio-examples/kroki"><img src="deploy-on-elestio.png" alt="Deploy on Elest.io" width="180px" /></a>

Deploy Kroki server with CI/CD on Elestio

<img src="kroki.png" style='width: 100%;'/>
<br/>
<br/>

# Once deployed ...

You can open Kroki UI here:

    URL: https://[CI_CD_DOMAIN]
    Login: "root"
    password: "[ADMIN_PASSWORD]"

# Diagram Generation with curl

This README provides instructions on how to generate diagrams using the curl command. Follow the steps below to create your diagrams effortlessly.

## Usage

To generate a new diagram, use the following curl command:

To generate a new diagram using curl command to this format:

    curl -o <PATH>/<YOUR_OUTPUT_FILE>.<EXTENSION_FILE> -X POST -u root:[ADMIN_PASSWORD] -H "Content-Type: application/json" -d '{
        "diagram_source": "digraph G {Hello->World}",
        "diagram_type": "graphviz",
        "output_format": "<OUTPUT_FORMAT>",
    }' https://[CI_CD_DOMAIN]/

Replace the placeholders with the appropriate values:

- `<PATH>`: The path where you want to store your file.
- `<YOUR_OUTPUT_FILE>`: The desired name of your output file.
- `<EXTENSION_FILE>`: The type of output format you desire, such as .svg, .png, .jpeg, or .pdf.
- `<OUTPUT_FORMAT>`: The format in which you want to generate the diagram (svg, png, jpeg, or pdf).

For example, to generate a PNG image named hello_world.png, run the following command:

    curl -o ./hello_world.png -X POST -u root:[ADMIN_PASSWORD] -H "Content-Type: application/json" -d '{
        "diagram_source": "digraph G {Hello->World}",
        "diagram_type": "graphviz",
        "output_format": "png",
    }' https://[CI_CD_DOMAIN]/

For more information: https://docs.kroki.io/kroki/setup/usage/

# Encoding Diagrams for GET Requests

When utilizing GET requests, your diagram needs to be encoded in the URL using a deflate + base64 algorithm.

For instance:

    https://[CI_CD_DOMAIN]/graphviz/svg/eNpLyUwvSizIUHBXqPZIzcnJ17ULzy_KSanlAgB1EAjQ

Will display a basic diagram of Hello->World

If you wish to encode your own diagram using Node.js, follow this example:

1.  install `pako` library

        npm install pako

2.  Create a file, for example, `kroki.js`, and add the following code:

        const pako = require('pako')

        const diagramSource = `digraph G {

    Hello->World
    }`

        const data = Buffer.from(diagramSource, 'utf8')
        const compressed = pako.deflate(data, { level: 9 })
        const result = Buffer.from(compressed)
        .toString('base64')
        .replace(/\+/g, '-')
        .replace(/\//g, '\_')

        console.log(result)

The `console.log()` will display the encoded result. You can then paste it into your URL as follows:

    https://[CI_CD_DOMAIN]/graphviz/svg/<YOUR_ENCODED_DIAGRAM>

## Kroki Documentation: Encoding Methods

Kroki offers various methods to encode your diagrams for GET requests. Below, you'll find a link to documentation explaining how to encode diagrams using different programming languages:

- Node.js
- JavaScript
- Python
- Java
- Kotlin
- Go
- PHP
- Tcl
- Elixir
- C#

https://docs.kroki.io/kroki/setup/encode-diagram/
