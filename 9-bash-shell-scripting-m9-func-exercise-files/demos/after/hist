#!/bin/bash

# This script prints a little histogram of how much space 
# the directories in the current working directory use

error () {
	echo "Error: $1"
	exit $2
} >&2

# Create a tempfile (in a BSD- and Linux-friendly way)
my_mktemp () {
	mktemp  || mktemp -t hist
} 2> /dev/null

# check we are using bash 4
(( BASH_VERSINFO[0] < 4 )) && error "This script can only be run by bash 4 or higher" 1

# An array to keep all the file sizes
declare -A file_sizes
declare -r tempfile=$(my_mktemp) || error "Cannot create tempfile" 2

# How wide is the terminal?
declare -ir term_cols=$(tput cols)

# Longest file name, Largest file, total file size
declare -i max_name_len=0 max_size=0 total_size=0

# A function to draw a line
drawline () {
	declare line=""
	declare char="-"
	for (( i=0; i<$1; ++i )); do
		line="${line}${char}"
	done
	printf "%s" "$line"
}

# This reads the output from du into an array
# And calculates total size and maximum size, max filename length
read_filesizes () {
	while read -r size name; do
		file_sizes["$name"]="$size"
		(( total_size += size ))
		(( max_size < size )) && (( max_size=size ))
		(( max_file_len < ${#name} )) && (( max_file_len=${#name} ))
	done
}

# run du to get filesizes
# Using a temporary file for output from du
{ du -d 0 */ || du --max-depth 0 *; } 2>/dev/null > "$tempfile"
read_filesizes <  "$tempfile"

# The length for each line and percentage for each file
declare -i length percentage
# How many columns may the lines take up?
declare -i cols="term_cols - max_file_len - 10"

for k in "${!file_sizes[@]}"; do
	(( length=cols * file_sizes[$k] / max_size ))
	(( percentage=100 * file_sizes[$k] / total_size ))
	printf "%-${max_file_len}s | %3d%% | %s\n" "$k" "$percentage" $(drawline $length)
done

printf "%d Directories\n" "${#file_sizes[@]}"
printf "Total size: %d blocks\n" "$total_size"

# clean up
rm "$tempfile"
exit 0
