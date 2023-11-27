#!/bin/bash

action=$(zenity --list --title="Firewall Management" --column="Action" --column="Description" \
        "Add" "Add a custom rule" \
        "Delete" "Delete a rule" \
        "Insert" "Insert a rule at a position" \
        "BlockSSH" "Block all SSH from a region" \
        "BlockAllIncoming" "Block all incoming traffic" \
        "BlockHTTP" "Block HTTP traffic" \
        "PreventDDoS" "Prevent DDoS attacks" \
        "ShowRules" "Display iptables rules" \
        "NetStats" "Show network statistics")

case $action in
    "Add")
        rule=$(zenity --entry --text "Enter custom iptables rule")
        if [ ! -z "$rule" ]; then
            sudo iptables $rule
        fi
        ;;
    "Delete")
        rule=$(zenity --entry --text "Enter rule number to delete")
        if [ ! -z "$rule" ]; then
            sudo iptables -D INPUT $rule
        fi
        ;;
    "Insert")
        position=$(zenity --entry --text "Enter position")
        rule=$(zenity --entry --text "Enter iptables rule")
        if [ ! -z "$rule" ] && [ ! -z "$position" ]; then
            sudo iptables -I INPUT $position $rule
        fi
        ;;
    "BlockSSH")
        region_ip_range=$(zenity --entry --title="Block SSH" --text="Enter IP range to block SSH" --entry-text "xxx.xxx.xxx.xxx/xx")
        sudo iptables -A INPUT -p tcp --dport 22 -s $region_ip_range -j DROP
        ;;
    "BlockAllIncoming")
        sudo iptables -P INPUT DROP
        sudo iptables -P FORWARD DROP
        ;;
    "BlockHTTP")
        sudo iptables -A INPUT -p tcp --dport 80 -j DROP
        ;;
    "PreventDDoS")
        sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j RETURN
        sudo iptables -A INPUT -p udp -m limit --limit 1/s --limit-burst 3 -j RETURN
        ;;
    "ShowRules")
        sudo iptables -L | zenity --text-info
        ;;
    "NetStats")
        netstat -tuwpln | zenity --text-info
        ;;
    *)
        zenity --info --text="No valid option selected"
        ;;
esac
