#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;


string trim(string text) {
    int first = 0;
    int last = text.length();
    last--;
    while (first <= last && (text[first] == ' ' || text[first] == '\t' || text[first] == '\r')) {
        first++;
    }
    while (last >= first && (text[last] == ' ' || text[last] == '\t' || text[last] == '\r')) {
        last--;
    }
    return text.substr(first, last - first + 1);
}

bool loadSongs(vector<string>& songs) {
    ifstream file("resources/songs.txt");
    if (!file.is_open()) {
        cout << "Could not open resources/songs.txt." << endl;
        return false;
    }
    string title;
    while (getline(file, title)) {
        title = trim(title);
        if (title != "" && count(songs.begin(), songs.end(), title) == 0) {
            songs.push_back(title);
        }
    }
    file.close();
    if (songs.empty()) {
        cout << "The songs file contains no song titles." << endl;
        return false;
    }
    return true;
}

int main() {
    vector<string> songs;
    if (!loadSongs(songs)) {
        return 1;
    }
    // Chapter 8 teaches random_shuffle; compile this program as C++11.
    random_shuffle(songs.begin(), songs.end());
    int nextSong = 0;
    int songCount = songs.size();
    cout << "Enter 'Play a random song' or 'quit'." << endl;
    string command;
    while (true) {
        cout << "> ";
        if (!getline(cin, command)) {
            break;
        }
        command = trim(command);
        if (command == "quit" || command == "Quit") {
            break;
        }
        if (command == "Play a random song") {
            if (nextSong == songCount) {
                cout << "All songs have been played. Starting a new cycle." << endl;
                random_shuffle(songs.begin(), songs.end());
                nextSong = 0;
            }
            // Every entry before nextSong has already played in this cycle.
            cout << "Now playing: " << songs[nextSong] << endl;
            nextSong++;
        } else {
            cout << "Unknown command. Enter 'Play a random song' or 'quit'." << endl;
        }
    }
    cout << "Goodbye!" << endl;
    return 0;
}
