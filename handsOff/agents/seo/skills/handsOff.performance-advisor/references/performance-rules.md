# Performance rules

Performance checks for ranking:
- Images MUST have width and height attributes (prevents layout shift).
- loading="lazy" for all images except first viewport.
- defer or async for all <script> tags.
- Critical path CSS should be identified.
