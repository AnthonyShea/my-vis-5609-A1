<script lang="ts">
  import * as d3 from "d3";
  import { onMount } from "svelte";
  import type { TMovie } from "../../types";
  import Bar from "$lib/Bar.svelte";
  import { RankedLine, GenreHeatmap } from '$lib';

  let movies: TMovie[] = [];

  async function loadCsv() {
    try {
      // The CSV should be in your static/ directory
      const csvUrl = "/my-vis-5609-A1/summer_movies.csv";

      const rawMovies = await d3.csv(csvUrl, (row) => {
        const year = +row.year;
        const runtime = +row.runtime_minutes;
        const rating = +row.average_rating;
        const votes = +row.num_votes;
        if (isNaN(year) || year < 1900 || year > 2025) return null; // Skip invalid years
        //if (isNaN(runtime) || isNaN(rating) || isNaN(votes)) return null; // Optional: skip other invalids
        return {
          tconst: row.tconst,
          title_type: row.title_type,
          primary_title: row.primary_title,
          original_title: row.original_title,
          year,
          runtime_minutes: runtime,
          genres: row.genres ? row.genres.split(',').map(g => g.trim()).filter(g => g) : [],
          simple_title: row.simple_title,
          average_rating: rating,
          num_votes: votes,
        };
      });

      movies = rawMovies.filter((m): m is TMovie => m !== null);

      console.log("Loaded CSV Data:", movies);
    } catch (error) {
      console.error("Error loading CSV:", error);
    }
  }

  onMount(loadCsv);
</script>

<h1>Summer Movies</h1>
<p>Here are {movies.length == 0 ? "..." : movies.length + " "} movies</p>
<Bar {movies} />

<h2>Q1: Annual Top 3 Genres Over Time (Top 8 Genres are labelled)</h2>
<RankedLine {movies} width={1000} height={400} />

<h2>Q2: Genre Co-Occurrences</h2>
<GenreHeatmap {movies} focusGenre="Comedy" width={800} height={600} />