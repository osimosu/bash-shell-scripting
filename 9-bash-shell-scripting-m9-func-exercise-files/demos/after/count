#/bin/bash

# This script prints a range of numbers

usage () {
	cat <<END
count [-r] [-b n] [-s n] stop

Print each number up to stop, beginning at 0
      -b: number to begin with (default: 0)
      -h: show this help message
      -r: reverses the count
      -s: sets step size (default: 1)
END
}

# function to handle errors.
# First argument: error message to print
# Second argument: exit code to exit script with
error () {
	echo "Error: $1"
	usage
	exit $2
} >&2

# Function returns 0 when it's argument is a number
isnum () {
	[[ $1 =~ ^[0-9]+$ ]]
}

declare reverse=""
declare -i begin=0
declare -i step=1

while getopts ":hb:s:r" opt; do
	case $opt in
		r)
			reverse="yes"			
			;;
		b) 
			isnum ${OPTARG} || error "${OPTARG} is not a number" 1
			start="${OPTARG}"
			;;
		h)
			usage
			exit 0
			;;
		s)
			isnum ${OPTARG} =~ ^[0-9]+$ || error "${OPTARG} is not a number" 1
			step="${OPTARG}"
			;;
		:) 
			error "Option -${OPTARG} is missing an argument" 2
			;;
		\?)
			error "Unknown option: -${OPTARG}" 3
			;;
	esac
done

shift $(( OPTIND -1 ))

[[ $1 ]] || error "missing an argument" 2
isnum $1 || error "$1 is not a number" 1
declare end="$1"

if [[ ! $reverse ]]; then
	for (( i=start; i <= end; i+=step )); do
		echo $i
	done
else
	for (( i=end; i >= start; i-=step )); do
		echo $i
	done
fi

exit 0

