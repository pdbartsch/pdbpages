---
title: "Q: How to quickly calculate turf per maintenance area?"
date: 2019-05-17T10:29:26-07:00
publishdate: 2019-05-17
lastmod: 2019-05-17
draft: false
tags: ['ArcGIS-Desktop', 'Python']
---

{{< figure src="/img/turfarea.png" title="Showing turf over maintenance areas." >}}

### A: Python in ArcGIS Pro:

{{< highlight python >}}

arcpy.analysis.SummarizeWithin("maintenance_area_pgon", "turf_area_gpon", r"path\to\save\output\file", "KEEP_ALL", None, "ADD_SHAPE_SUM", "SQUAREFEET", None, "NO_MIN_MAJ", "NO_PERCENT", r"path\to\save\group\table")
{{< /highlight >}}

The parameters for Summarize Within are 
{{< highlight python >}}
SummarizeWithin_analysis (in_polygons, in_sum_features, out_feature_class, {keep_all_polygons}, {sum_fields}, {sum_shape}, {shape_unit}, {group_field}, {add_min_maj}, {add_group_percent}, {out_group_table})
{{< /highlight >}}

For the full docs have a look at [esri's documentation](https://pro.arcgis.com/en/pro-app/tool-reference/analysis/summarize-within.htm).

