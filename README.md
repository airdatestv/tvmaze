# tvmaze
If you want to create a PHP script that fetches data from the TVMaze API and outputs it as HTML, you'll need to use PHP's file_get_contents or cURL to make API requests.
PHP Script: Fetching Data from TVMaze API and Displaying It in HTML
<?php
// TVMaze API URL
$tvmazeApiUrl = 'https://api.tvmaze.com/shows/82'; // Example show ID: 82 (e.g., "Breaking Bad")

// Function to fetch data from TVMaze API
function fetchTvMazeData($url) {
    $response = file_get_contents($url);
    if ($response === FALSE) {
        return null;
    }
    return json_decode($response, true);
}

// Fetch data from TVMaze
$showData = fetchTvMazeData($tvmazeApiUrl);

// Check if data was successfully fetched
if ($showData === null) {
    echo '<p>Error fetching data from TVMaze.</p>';
} else {
    // Extract data fields
    $showName = htmlspecialchars($showData['name']);
    $showSummary = htmlspecialchars($showData['summary']);
    $showImage = htmlspecialchars($showData['image']['original']);
    $showGenres = implode(', ', array_map('htmlspecialchars', $showData['genres']));
    $showLanguage = htmlspecialchars($showData['language']);
    $showRuntime = htmlspecialchars($showData['runtime']);
    $showPremiered = htmlspecialchars($showData['premiered']);
    $showRating = htmlspecialchars($showData['rating']['average']);
    $showOfficialSite = htmlspecialchars($showData['officialSite']);
    
    // Output HTML
    echo '<!DOCTYPE html>';
    echo '<html lang="en">';
    echo '<head>';
    echo '<meta charset="UTF-8">';
    echo '<meta name="viewport" content="width=device-width, initial-scale=1.0">';
    echo '<title>' . $showName . '</title>';
    echo '<style>';
    echo 'body { font-family: Arial, sans-serif; line-height: 1.6; margin: 20px; }';
    echo 'img { max-width: 100%; height: auto; }';
    echo '.show-info { margin-bottom: 20px; }';
    echo '</style>';
    echo '</head>';
    echo '<body>';
    echo '<h1>' . $showName . '</h1>';
    echo '<div class="show-info">';
    echo '<img src="' . $showImage . '" alt="' . $showName . '">';
    echo '<p><strong>Genres:</strong> ' . $showGenres . '</p>';
    echo '<p><strong>Language:</strong> ' . $showLanguage . '</p>';
    echo '<p><strong>Runtime:</strong> ' . $showRuntime . ' minutes</p>';
    echo '<p><strong>Premiered:</strong> ' . $showPremiered . '</p>';
    echo '<p><strong>Rating:</strong> ' . $showRating . '/10</p>';
    echo '<p><strong>Official Site:</strong> <a href="' . $showOfficialSite . '" target="_blank">' . $showOfficialSite . '</a></p>';
    echo '</div>';
    echo '<div class="show-summary">';
    echo '<h2>Summary</h2>';
    echo '<p>' . $showSummary . '</p>';
    echo '</div>';
    echo '</body>';
    echo '</html>';
}
?>
Explanation
API URL: The script uses a hardcoded URL to fetch details for a specific show ID (82 in this example). You can change this to any valid TVMaze show ID.

Function to Fetch Data:

fetchTvMazeData($url): This function uses file_get_contents to fetch data from the given URL and decodes the JSON response into an associative array. You could use cURL for more advanced needs, such as handling different HTTP methods or timeouts.
Data Extraction:

After fetching the data, the script extracts various fields like the show's name, summary, image, genres, language, runtime, premiered date, rating, and official site.
htmlspecialchars is used to escape any special HTML characters to prevent XSS attacks.
HTML Output:

The script constructs a simple HTML page with inline CSS for basic styling.
The img tag displays the show's image, and various details about the show are presented in paragraphs.
Error Handling:

If the data cannot be fetched, an error message is displayed.
Additional Notes
Security: Make sure to sanitize and validate inputs if you extend the script to accept dynamic show IDs.
Error Handling: Consider adding more robust error handling, such as checking HTTP response codes and handling different types of API errors.
cURL Alternative: For more control over the HTTP request, you could replace file_get_contents with a cURL request.
This script provides a basic example of how to interact with an external API and present the data using PHP and HTML. Adjust and expand upon this as needed for your application.
