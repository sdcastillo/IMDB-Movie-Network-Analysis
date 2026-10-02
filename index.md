---
layout: default
title: IMDB Movie Network Analysis
description: Films linked by IMDb recommendations. The drawing that made connected representations click.
samwiki: true
---

<div class="sw-lede">
  <div class="sw-lede-copy">
    <h2>Where the connections started to mean something</h2>
    <p>This is the project where Sam Castillo found his way into neural networks and large language models. It did not start as a model. It started as a drawing of movies.</p>
    <p>On an IMDb title page there is a short list: people who liked this also liked about twelve other films. Sam took a Kaggle table of about 5,000 movies, followed those lists, and drew a line whenever one film recommended another. A movie stopped being only a row of year, budget, and score. It became a point with neighbors.</p>
    <p>Keywords make the same kind of picture from the other direction. Two films can sit close because they share plot words, even when neither page names the other. Recommendations and keywords are both ways of saying that a film is partly made of the films around it.</p>
    <p>That is the click. A neural network and a language model also keep meaning in the links: a word, a sentence, or a film is a position among other positions, not a sealed box. The later math is heavier. The idea was already here, in a homemade map of who points to whom.</p>
  </div>
  <aside class="sw-find">
    <h2>What the picture is</h2>
    <ul>
      <li><strong>A point</strong> One film from the IMDb 5000 table, after incomplete rows are dropped.</li>
      <li><strong>A line</strong> One of the twelve “People who liked this also liked” links on a title page.</li>
      <li><strong>A color</strong> The first genre listed for that film, drawn with the Set1 palette.</li>
      <li><strong>A matrix</strong> 1 when film <em>i</em> recommends film <em>j</em> inside the slice, and 0 otherwise.</li>
      <li><strong>The notes</strong> <a href="IMDB_network_final.pdf">IMDB_network_final.pdf</a> and the R Markdown beside it.</li>
    </ul>
  </aside>
</div>

## The drawings

<p>These are the network files already in the repository. The PNG is embedded as it was saved. The two TIFF drawings live in <a href="larger%20networks.7z"><code>larger networks.7z</code></a> (<code>big_network.tiff</code>, 5330×3740, and <code>Rplot03.tiff</code>, 10000×5000). Browsers do not show TIFF, so each one is also here as a trimmed PNG and WebP. The archive is still the original.</p>

<figure>
  <a href="Network_genre_cluster.png">
    <img src="Network_genre_cluster.png" alt="Colored network of films linked by IMDb recommendations, from Network_genre_cluster.png." width="1274" height="718">
  </a>
  <figcaption>
    <code>Network_genre_cluster.png</code>. Films are the points and IMDb recommendations are the lines. Color follows the first genre on the title, the same coding as the R notes. Labels on the drawing include titles such as Jane Got a Gun, Alice Through the Looking Glass, and Hotel Transylvania 2.
    <a href="Network_genre_cluster.png">Open the PNG</a>.
  </figcaption>
</figure>

<figure>
  <picture>
    <source srcset="assets/figures/big_network.webp" type="image/webp">
    <img src="assets/figures/big_network.png" alt="Grayscale movie-network drawing converted from big_network.tiff." width="1600" height="1087">
  </picture>
  <figcaption>
    <code>big_network.tiff</code>, drawn large and kept in grayscale: white page, gray links, black points. The file above is a trimmed copy at 1600 pixels wide so the page can show it.
    <a href="assets/figures/big_network.png">PNG</a>,
    <a href="assets/figures/big_network.webp">WebP</a>,
    original in <a href="larger%20networks.7z">larger networks.7z</a>.
  </figcaption>
</figure>

<figure>
  <picture>
    <source srcset="assets/figures/Rplot03.webp" type="image/webp">
    <img src="assets/figures/Rplot03.png" alt="Grayscale network plot converted from Rplot03.tiff." width="1800" height="895">
  </picture>
  <figcaption>
    <code>Rplot03.tiff</code>, an R plot of the same kind of network, 10000 by 5000 pixels, also grayscale. The page shows a trimmed copy. The TIFF itself stays in the archive.
    <a href="assets/figures/Rplot03.png">PNG</a>,
    <a href="assets/figures/Rplot03.webp">WebP</a>,
    original in <a href="larger%20networks.7z">larger networks.7z</a>.
  </figcaption>
</figure>

## How a recommendation becomes a line

<p>Each IMDb title page carries a recommendation panel. Under a film such as <a href="https://www.imdb.com/title/tt0111161/">The Shawshank Redemption</a>, the section “People who liked this also liked” offers a window of about twelve other movies. This project turns that panel into a network. It starts from the <a href="https://www.kaggle.com/deepmatrix/imdb-5000-movie-dataset">IMDb 5000 Movie Dataset</a> on Kaggle and, for each title kept in the working table, scrapes those recommended links. A movie becomes a node, and a recommendation becomes a chance at an edge.</p>

<p>The Kaggle extract is full of gaps. Gross, budget, and several other columns are blank often enough that a complete-case model would discard a large share of the catalog. The plot needs a set of titles and the links between them, so rows with missing values are dropped and the table is cut down to gross, genres, title, country, the IMDb URL, budget, release year, score, and content rating. Each film keeps the first genre in its list, which gives the node a single color, and the title text is cleaned before it is used as a label.</p>

<p>Gross and budget are restated in 2016 dollars. A consumer-price index is joined on release year, and each film’s dollars are scaled by the ratio of the 2016 index to the index for its own year. The network is limited to films made in the United States, so a year slice compares titles from one production country.</p>

<p>The scraper reads a movie page, keeps the lines that contain <code>rec_item</code>, and pulls the IMDb title constant out of each recommendation slot. Those constants are rebuilt as title URLs. For a chosen slice, an adjacency matrix stores 1 when film <em>i</em> recommends film <em>j</em> and 0 otherwise. <code>ggnet2</code> draws that matrix as an undirected network. Color comes from the simplified genre and RColorBrewer’s Set1 palette, which is why the plot holds at most nine genres: Action, Adventure, Animation, Comedy, Crime, Drama, Fantasy, Horror, and Biography. Labeling and node size are arguments, so a crowded slice can drop the names and shrink the points.</p>

<p>The slices show how much of that recommendation structure sits inside a window of years. Releases from 2016 onward leave a small U.S. graph, about forty-five films, with titles still readable. The 1970s graph is sparse because many of the twelve recommendations for each film fall outside the decade, so the in-slice matrix is thin. Films from before 1975 are thinner still, and the edges that remain tend to join movies released near one another, sequels among them. A 2006–2007 slice is too dense for labels. With the names removed, the layout shows whether genre, along with year, pulls the nodes into groups. The notes title that crowded slice “Movies from 2010–2014”; the code keeps U.S. films with a release year after 2005 and before 2008. The drawn slices are saved in <a href="IMDB_network_final.pdf">IMDB_network_final.pdf</a>.</p>

<ul>
  <li><strong>Source.</strong> The IMDb 5000 Movie Dataset, after dropping incomplete rows.</li>
  <li><strong>Edge.</strong> One of the twelve “People who liked this also liked” links on a title page.</li>
  <li><strong>Matrix.</strong> 1 when film <em>i</em> recommends film <em>j</em> inside the slice, and 0 otherwise.</li>
  <li><strong>Drawing.</strong> An undirected <code>ggnet2</code> network, colored with Set1 by the first-listed genre.</li>
  <li><strong>Dollars.</strong> Gross and budget scaled to 2016 with the CPI, U.S. releases only.</li>
  <li><strong>Windows.</strong> 2016 and later, the 1970s, before 1975, and 2006–2007.</li>
</ul>

<p class="sw-actions">
  <a class="sw-btn sw-btn-live" href="IMDB_network_final.pdf">Read the notes (PDF)</a>
  <a class="sw-btn sw-btn-source" href="https://github.com/sdcastillo/IMDB-Movie-Network-Analysis">Source</a>
  <a class="sw-btn sw-btn-source" href="larger%20networks.7z">Original TIFFs (.7z)</a>
</p>
