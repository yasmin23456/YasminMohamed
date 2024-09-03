#!/bin/bash
moduser=$(whiptail --inputbox "Please enter the username:" 8 39 3>&1 1>&2 2>&3)
        if [ $? != 0 ] || [ -z "$moduser" ]; then
            echo "Operation cancelled. Exiting..."
            exit 1
        fi
# Check if the user exists
    if id "$moduser" &>/dev/null; then
        options=$(whiptail --title "Modify User $moduser" --menu "Please select what you want to do for user $moduser" 15 60 6 \
        "1" "Allow bad names" \
        "2" "New home directory for the user account" \
        "3" "Force use GROUP as new primary group" \
        "4" "Lock the user account" \
        "5" "New UID for the user account" \
        "6" "Append the user to new groups" \
        3>&1 1>&2 2>&3)

        if [ $? != 0 ]; then
            echo "Operation cancelled. Exiting..."
            exit 1
        fi

        case $options in
            "1")
                echo "You chose to allow bad names for user $moduser."
                sudo usermod -b "$moduser"
                ;;
            "2")
                new_home=$(whiptail --inputbox "Enter new home directory for $moduser:" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$new_home" ]; then
                    echo "Setting new home directory for user $moduser to $new_home."
                    sudo usermod -d "$new_home" "$moduser"
                else
                    echo "Operation cancelled or invalid input for home directory."
                fi
                ;;
            "3")
                group=$(whiptail --inputbox "Enter new primary group for $moduser:" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$group" ]; then
                    echo "Setting new primary group for user $moduser to $group."
                    sudo usermod -g "$group" "$moduser"
                else
                    echo "Operation cancelled or invalid input for group."
                fi
                ;;
            "4")
                echo "Locking the account for user $moduser."
                sudo usermod -L "$moduser"
                ;;
            "5")
                uid=$(whiptail --inputbox "Enter new UID for $moduser:" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$uid" ]; then
                    echo "Setting new UID for user $moduser to $uid."
                    sudo usermod -u "$uid" "$moduser"
                else
                    echo "Operation cancelled or invalid input for UID."
                fi
                ;;
            "6")
                groups=$(whiptail --inputbox "Enter groups to append $moduser to (comma-separated):" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$groups" ]; then
                    echo "Appending user $moduser to groups: $groups."
                    sudo usermod -a -G "$groups" "$moduser"
                else
                    echo "Operation cancelled or invalid input for groups."
                fi
                ;;
            *)
                echo "Invalid option selected. Exiting..."
                ;;
        esac
else 
whiptail --msgbox "User $moduser does not exist. Please choose another username." 8 39
    fi
