#!/bin/bash

modgroup=$(whiptail --inputbox "Please enter the group name to modify:" 8 39 3>&1 1>&2 2>&3)
        if [ $? != 0 ] || [ -z "$modgroup" ]; then
            echo "Operation cancelled. Exiting..."
            exit 1
        fi
#Check if the group exists
if getent group "$modgroup" > /dev/null; then
        options=$(whiptail --title "Modify Group $modgroup" --menu "Please select what you want to do for group $modgroup" 15 60 6 \
        "1" "Change group name" \
        "2" "Add users to group" \
        "3" "Remove users from group" \
        3>&1 1>&2 2>&3)

        if [ $? != 0 ]; then
            echo "Operation cancelled. Exiting..."
            exit 1
        fi

        case $options in
            "1")
                new_name=$(whiptail --inputbox "Enter new name for group $modgroup:" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$new_name" ]; then
                    sudo groupmod -n "$new_name" "$modgroup"
                    whiptail --msgbox "Group $modgroup has been renamed to $new_name." 8 39
                else
                    echo "Operation cancelled or invalid input for group name."
                fi
                ;;
            "2")
                users=$(whiptail --inputbox "Enter users to add to $modgroup (comma-separated):" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$users" ]; then
		 sudo usermod -a -G "$modgroup" "$users"
                    whiptail --msgbox "Users $users have been added to group $modgroup." 8 39
                else
                    echo "Operation cancelled or invalid input for users."
                fi
                ;;
            "3")
                users=$(whiptail --inputbox "Enter users to remove from $modgroup (comma-separated):" 8 39 3>&1 1>&2 2>&3)
                if [ $? = 0 ] && [ ! -z "$users" ]; then
                    for user in $(echo "$users" | tr "," " "); do
                        sudo gpasswd -d "$user" "$modgroup"
                    done
                    whiptail --msgbox "Users $users have been removed from group $modgroup." 8 39
                else
                    echo "Operation cancelled or invalid input for users."
                fi
                ;;
            *)
                echo "Invalid option selected. Exiting..."
                ;;
       esac
else
        whiptail --msgbox "Group $modgroup does not exist. Please choose another group." 8 39
fi

