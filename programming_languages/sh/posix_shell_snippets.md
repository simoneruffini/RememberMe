# POSIX shell snippets

## Usefull Functions
```sh
# Check if an input variable is a number (in base 10)
is_integer ()
{
    case "${1#[-]}" in
        (*[!0123456789]*) return 1 ;;
        ('')              return 1 ;;
        (*)               return 0 ;;
    esac
}
```

## Get Options Arguments
Get options arguments from shell. 
```sh
OPTIND=1         # Reset in case getopts has been used previously in the sh

args="d:" 		# Input db path (semicolon means option has an arugment)
args="${args} c:"	# Config file path
args="${args} h" 	# Help 

while getopts "$args" opt; do
	case "$opt" in 
		h)
			Help
			exit 0
			;;
		d)
			db_path=$OPTARG
			;;
		c)
			cfg_path=$OPTARG
			;;
	esac
done
shift $((OPTIND-1))
```
