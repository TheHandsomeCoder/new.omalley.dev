---
title: "{{ replaceRE `^[0-9]{8}-` "" .File.ContentBaseName | humanize | title }}"
slug: "{{ replaceRE `^[0-9]{8}-` "" .File.ContentBaseName }}"
date: "{{ .Date }}"
draft: false
---