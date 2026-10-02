---
layout: default
title: IMDB Movie Network Analysis
description: A network plot of films joined by IMDb's recommended-movie links.
samwiki: true
---

Each IMDb title page carries a recommendation panel. Under a film such as [The Shawshank Redemption](https://www.imdb.com/title/tt0111161/), the section "People who liked this also liked" offers a window of about twelve other movies. This project turns that panel into a network. It starts from the [IMDb 5000 Movie Dataset](https://www.kaggle.com/deepmatrix/imdb-5000-movie-dataset) on Kaggle and, for each title kept in the working table, scrapes those recommended links. A movie becomes a node, and a recommendation becomes a chance at an edge.

The Kaggle extract is full of gaps. Gross, budget, and several other columns are blank often enough that a complete-case model would discard a large share of the catalog. The plot needs a set of titles and the links between them, so rows with missing values are dropped and the table is cut down to gross, genres, title, country, the IMDb URL, budget, release year, score, and content rating. Each film keeps the first genre in its list, which gives the node a single color, and the title text is cleaned before it is used as a label.

Gross and budget are restated in 2016 dollars. A consumer-price index is joined on release year, and each film's dollars are scaled by the ratio of the 2016 index to the index for its own year. The network is limited to films made in the United States, so a year slice compares titles from one production country.

The scraper reads a movie page, keeps the lines that contain `rec_item`, and pulls the IMDb title constant out of each recommendation slot. Those constants are rebuilt as title URLs. For a chosen slice, an adjacency matrix stores 1 when film *i* recommends film *j* and 0 otherwise. `ggnet2` draws that matrix as an undirected network. Color comes from the simplified genre and RColorBrewer's Set1 palette, which is why the plot holds at most nine genres: Action, Adventure, Animation, Comedy, Crime, Drama, Fantasy, Horror, and Biography. Labeling and node size are arguments, so a crowded slice can drop the names and shrink the points.

The slices show how much of that recommendation structure sits inside a window of years. Releases from 2016 onward leave a small U.S. graph, about forty-five films, with titles still readable. The 1970s graph is sparse because many of the twelve recommendations for each film fall outside the decade, so the in-slice matrix is thin. Films from before 1975 are thinner still, and the edges that remain tend to join movies released near one another, sequels among them. A 2006–2007 slice is too dense for labels. With the names removed, the layout shows whether genre, along with year, pulls the nodes into groups. The drawn slices are saved in [IMDB_network_final.pdf](IMDB_network_final.pdf).

- Source: the IMDb 5000 Movie Dataset, after dropping incomplete rows.
- Edge: one of the twelve "People who liked this also liked" links on a title page.
- Matrix: 1 when film *i* recommends film *j* inside the slice, and 0 otherwise.
- Drawing: an undirected `ggnet2` network, colored with Set1 by the first-listed genre.
- Dollars: gross and budget scaled to 2016 with the CPI, U.S. releases only.
- Windows: 2016 and later, the 1970s, before 1975, and 2006–2007.
