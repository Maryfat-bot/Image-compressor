---
name: image-compressor
description: Batch optimize and resize JPG, PNG, SVG, and GIF images while preserving visual quality; use for local image compression, size reduction, and format-aware optimization.
metadata:
  short-description: Batch compress images with quality preservation
---

# Image compressor

Optimize local image files in batches, using format-aware tools and preserving originals. Aim for the smallest practical files while retaining the requested visual quality. The iLoveIMG reference workflow supports multiple files, drag-and-drop style batch selection, and a single compression action; apply the same convenience locally without uploading files unless the user explicitly requests an online service.

## Workflow

1. Identify inputs, formats, output location, and any requested maximum dimensions or file size. If the user asks only to compress, keep dimensions unchanged.
2. Inspect available local tools before choosing an approach. Prefer established utilities (`jpegoptim`/`mozjpeg`, `oxipng`/`optipng`, `svgo`, `gifsicle`, ImageMagick, or equivalent). Do not install packages without the user's request.
3. Process every supported input. Keep originals untouched; write results to a sibling `compressed/` directory or a user-specified destination. Preserve filenames where possible; avoid overwriting existing files.
4. Use lossless optimization by default. JPEG recompression is lossy even at high quality; only re-encode when needed and choose a quality-preserving setting. Preserve alpha, animation, frame timing, orientation, and color profile where supported. SVG minification must preserve rendering and referenced assets.
5. For resizing, apply the requested dimensions consistently while preserving aspect ratio unless the user specifies otherwise. For animated GIF, retain animation; do not silently flatten.
6. Compare output sizes and verify files can be decoded. Summarize processed, skipped, and failed files, and report before/after sizes and savings. If a format/tool cannot preserve a property, disclose it and keep the original.

## Format guidance

- **JPG/JPEG:** lossless metadata cleanup/optimization first. Re-encoding changes pixels; use a conservative quality (typically 90–95) and preserve chroma quality. Do not call JPEG re-encoding lossless.
- **PNG:** use lossless optimizers. Avoid palette conversion or quantization unless the user explicitly accepts color loss.
- **SVG:** use SVGO or an equivalent structural minifier. Preserve viewBox, IDs used by CSS/scripts, accessibility labels, embedded data, and external references as needed; avoid converting text or shapes.
- **GIF:** use lossless frame optimization. Maintain frame count, delay, loop behavior, transparency, and disposal settings. Color quantization is lossy and requires explicit user acceptance.

## Batch helper

For repeatable batch processing, use `scripts/compress_images.py`. It runs installed command-line optimizers only, never installs dependencies, and writes into a separate output directory. Consult `references/iloveimg-feature-reference.md` for the product features that informed this skill.
