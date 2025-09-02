<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Movie Collection Manager</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .movie-card {
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .movie-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .fade-in {
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen font-sans">
    <header class="bg-gradient-to-r from-blue-600 to-purple-600 text-white py-6 px-4">
        <h1 class="text-3xl font-bold text-center">Prabhat Movie Collection</h1>
        <p class="text-center mt-2">Manage, track, and discover your favorite films
        </p>
    </header>

    <main class="container mx-auto px-4 py-8">
        <!-- Add Movie Section -->
        <section class="bg-white rounded-lg shadow-lg p-6 mb-8 fade-in">
            <h2 class="text-2xl font-semibold mb-4">Add a New Movie</h2>
            <form id="movie-form" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <input type="text" id="movie-image" placeholder="Movie Poster URL (e.g., https://example.com/poster.jpg)" class="border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                <input type="text" id="movie-name" placeholder="Movie Name" class="border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" required>
                <input type="number" id="movie-rating" placeholder="IMDb Rating (e.g., 8.5)" step="0.1" min="0" max="10" class="border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                <input type="number" id="movie-year" placeholder="Release Year" min="1900" max="2030" class="border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                <textarea id="movie-description" placeholder="Brief description or plot summary" rows="3" class="border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 col-span-1 md:col-span-2"></textarea>
                <button type="submit" class="bg-blue-600 text-white px-6 py-3 rounded-md hover:bg-blue-700 transition duration-300 col-span-1 md:col-span-2">Add Movie</button>
            </form>
        </section>

        <!-- Search Section -->
        <section class="mb-6">
            <input type="text" id="search-input" placeholder="Search movies by name..." class="w-full border border-gray-300 p-3 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
        </section>

        <!-- Movie List -->
        <section id="movie-list" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Movies will be rendered here -->
        </section>
    </main>

    <script>
        let movies = JSON.parse(localStorage.getItem('movies')) || [];

        // Render movies
        function renderMovies(filteredMovies = movies) {
            const movieList = document.getElementById('movie-list');
            movieList.innerHTML = '';

            filteredMovies.forEach((movie, index) => {
                const card = document.createElement('div');
                card.className = 'movie-card bg-white rounded-lg shadow-lg overflow-hidden fade-in';
                card.innerHTML = `
                    <img src="${movie.image || 'https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/702f5e56-f5af-4dcf-8c25-7d9d87dfefd4.png'}" alt="Poster of ${movie.name} - a ${movie.year} film with IMDb rating ${movie.rating} out of 10" class="w-full h-64 object-cover" onerror="this.src='https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/7ac1dc46-f5c1-4d70-842e-cc03b1d2b067.png'">
                    <div class="p-4">
                        <h3 class="text-xl font-bold mb-2">${movie.name}</h3>
                        <p class="text-gray-600 mb-1"><strong>Year:</strong> ${movie.year || 'N/A'}</p>
                        <p class="text-gray-600 mb-1"><strong>IMDb Rating:</strong> ${movie.rating || 'N/A'}/10</p>
                        <p class="text-gray-700 mb-3">${movie.description || 'No description available.'}</p>
                        <label class="flex items-center mb-3">
                            <input type="checkbox" ${movie.watched ? 'checked' : ''} onchange="toggleWatched(${index})" class="mr-2">
                            Watched
                        </label>
                        <button onclick="deleteMovie(${index})" class="bg-red-500 text-white px-4 py-2 rounded-md hover:bg-red-600 transition duration-300">Delete</button>
                    </div>
                `;
                movieList.appendChild(card);
            });
        }

        // Add movie
        document.getElementById('movie-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const image = document.getElementById('movie-image').value;
            const name = document.getElementById('movie-name').value;
            const rating = document.getElementById('movie-rating').value;
            const year = document.getElementById('movie-year').value;
            const description = document.getElementById('movie-description').value;

            movies.push({ image, name, rating, year, description, watched: false });
            localStorage.setItem('movies', JSON.stringify(movies));
            renderMovies();

            // Clear form
            document.getElementById('movie-form').reset();
        });

        // Toggle watched
        function toggleWatched(index) {
            movies[index].watched = !movies[index].watched;
            localStorage.setItem('movies', JSON.stringify(movies));
        }

        // Delete movie
        function deleteMovie(index) {
            movies.splice(index, 1);
            localStorage.setItem('movies', JSON.stringify(movies));
            renderMovies();
        }

        // Search movies
        document.getElementById('search-input').addEventListener('input', function() {
            const query = this.value.toLowerCase();
            const filtered = movies.filter(movie => movie.name.toLowerCase().includes(query));
            renderMovies(filtered);
        });

        // Initial render
        renderMovies();
    </script>
</body>
</html>
</content>
</create_file>
</thinking>
<attempt_completion>
I've created a responsive, interactive movie collection website in a single HTML file. It includes:

- A form to add movies with fields for image URL, name, IMDb rating, release year, and description
- A search bar to filter movies by name
- Movie cards displaying all details, a "Watched" checkbox for the checklist feature, and a delete button
- Data is stored in local storage, so your collection persists
- Modern UI with Tailwind CSS, including hover animations and a gradient header
- Responsive design that works on mobile, tablet, and desktop
- Fallback image handling for missing posters

To use it, just open the file in a browser. Add movies by filling the form, and your collection will build up with search and wishlist tracking capabilities. If you need any adjustments or additions, let me know!
</attempt_completion>
<script>
function normalizeMovie(m) {
  return {
    image: (m.image || m.poster || m.posterUrl || m.img || '').trim(),
    name:  (m.name  || m.title  || m.movieName || 'Untitled').trim(),
    rating: (m.rating || m.imdbRating || m.imdb || '').toString(),
    year:   (m.year   || m.releaseYear || '').toString(),
    description: (m.description || m.plot || m.desc || '').trim(),
    watched: !!m.watched
  };
}

let movies = (JSON.parse(localStorage.getItem('movies')) || []).map(normalizeMovie);
localStorage.setItem('movies', JSON.stringify(movies));
