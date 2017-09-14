---
excerpt: "Detailed rundown of my Home Media Server"
header:
  overlay_color: "#333"
categories:
  - Guides 
tags:
  - Personal
---
# Home Media Server build

I've been asked a couple of times by different people about my home server, so I decided to write up a short article talking about the process that went into making it and setting up the various pieces of software in use.

This post is a Work In Progress and will be updated soon.

# Table of Contents
* Table of Contents
{:toc}

# Hardware

The server currently resides in a Fractal Design Define R3 case. This case supports up to 8 3.5" drives and has noise dampening foam surrounding it, meaning the server is near inaudible under normal load.

The processor currently used is a Celeron G1620, sitting in an ASRock B75M mATX motherboard. This CPU is surprisingly powerful for what it cost 4 years ago. It appears to be able to handle two concurrent 1080p streams no problem, but in my house usually only one person is using the server at a time.

Storage is spread across a set of 3 3TB Western Digital RED drives. I do not use hardware RAID in this server, instead opting for a software solution via unRAID.

# Software

## unRAID

The operating system in use on the server is unRAID, a non-free paid OS designed to provide NAS functions, alongside virtualized and containerized application services.

This was chosen due to simplicity of setup and use compared to something like MergerFS, SnapRAID, etc. In the future I might migrate to a MergerFS/SnapRAID setup just for the experience.

## NZBGet

## Sonarr

## Radarr

## Plex Media Server
