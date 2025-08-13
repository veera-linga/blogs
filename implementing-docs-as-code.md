# Implementing Docs as Code
## Background

Our documentation system previously relied on a **commercial XML source control, authoring, and publishing suite**.  
The workflow included XML-based authoring in an editor and publishing HTML/PDF outputs using a dedicated publishing pipelines.

While the system was robust and feature-rich, it introduced limitations:

- **Limited ownership** – The platform was controlled externally, restricting our ability to customize workflows or add features.  
- **Dependency for metrics** – Accessing performance analytics required vendor assistance.  
- **Feature bottlenecks** – Introducing new functionality was slow and inflexible.

To address these issues, a new **Docs as Code** strategy was designed, leveraging open-source tools, version control, and cloud delivery pipelines.

## Strategy Overview

The goal was to unify tools, reduce dependencies, and bring full control of the documentation workflow in-house utilizing the same principles, tools, and workflows as software code.

## Example Workflow Diagram

<div class="mermaid">
%%{init: {'theme':'base', 'flowchart': {'layout': 'elk'}}}%%
flowchart LR
    A(["XML Source Files in Git"]) -- "DITA-OT Markdown Export" --> B["Markdown Files"]
    B -- Pandoc --> C["RST Index Files"]
    C -- Sphinx Build --> D["Custom HTML/CSS Theme"]
    C2(["Docs metadata in JSON"]) -- "script to inject meta to HTMLs" --> D
    D -- Deploy --> E["Object Storage Bucket"]
    E -- CDN Distribution --> F["End Users"]
</div>

Two approaches were considered:

**Option 1 – Direct HTML generation with DITA-OT**  
- Use DITA-OT’s built-in arguments instead of custom plugins. For example, `args.hdf`, `args.hdr`, `args.ftr`, and `args.css`. 
- Apply custom CSS, metadata, and structural adjustments through configuration parameters.  
- Reduced complexity compared to maintaining custom publishing plugins.

**Option 2 – XML to Markdown, then Markdown to HTML with Sphinx**  
- Use DITA-OT to convert XML sources to Markdown.  
- Process Markdown files with Sphinx for final HTML rendering.  
- Aligned all source files into a lightweight, Git-friendly format and simplified future tooling changes.

**Decision:** The second option was chosen for its simplicity, transparency, and ease of integrating automation.

## Final Implementation

### 1. Converting XML to markdown
```bash
dita --input=my_ditamap.ditamap --format=markdown --output=./output_folder
````

### 2. Preparing Sphinx inputs

```bash
pandoc --from=markdown --to=rst --output=output.rst input.md
```

### 3. Rendering HTMLs with Sphinx

* Customized the Sphinx theme to match production site styling.
* Applied CSS and HTML overrides to minimize visible changes.

### 4. Injecting metadata to HTMLs
* Guides metadata is saved in a JSON file, which includes details such as the creation date, modified date, author name, versions, document template, and other crucial details.
* Use script to inject the metadata to each HTML.

### 5. Hosting and distribution

* Published HTML pages to an object storage bucket.
* Configured WAF.
* Distributed via CDN for high availability and low latency.
* Updated DNS to switch production domains to the new pipeline.

## Benefits

* **Cost savings** – Over a half-million USD in annual savings on tool licenses.
* **Full control of tooling** – No vendor lock-in or dependencies.
* **Version-controlled content** – Transparent history and collaboration in Git.
* **Reduced complexity** – Unified tools and documentation operations.
* **Future extensibility** – Easy to add validations, linters, AI-assisted authoring and many more.

## Learning

* Start with a proof-of-concept before a full migration.
* DITA-OT custom plugins are complex and buggy; use in-built DITA parameters for customization instead. 
* Use scripts to automate everything possible.
* Move the source code and scripts to Git for testing instead of relying on a local environment. This helps with collaboration and enables wider testing scenarios.
* Document your scripts and pipelines for clarity and quick onboarding.


## What’s Next

* Add automated validations for formatting, link integrity, and terminology.
* Explore AI-assisted authoring and review agents.
* Build an internal analytics dashboard.

## References

* [DITA Open Toolkit Parameters](https://www.dita-ot.org/dev/parameters/parameters-html5#html5)
* [Pandoc](https://pandoc.org/)
* [Sphinx Documentation](https://www.sphinx-doc.org/)

## Keywords

`Adobe Experience Manager` `Adobe FrameMaker` `Robohelp` `EasyDita` `Oxygen XML editor` `ArborText XML Editor` `gitlab` `XML` `XSLT` `Markdown` `RST` `AsciiDoc` `mermaid charts` `DITA` `DITA-OT` `pandoc` `Sphinx` `Python` `JavaScript` `docops` `CICD` `vibecoding` `GenAI` `AWS S3` `cloudfront` `distributed cloud` `WAF` `DNS` `version control` `Docs-as-Code` `Content Delivery Network` `Content Management System` `technical writing` `customer documentation` `customer communications` 
