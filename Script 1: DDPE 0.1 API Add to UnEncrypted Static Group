#!/bin/bash

############################################################
#API Credentials
############################################################
apiuser=$4
apipass=$5
jssURL=$6

############################################################
#Obtain Serial Number from End User Device
############################################################
SERIAL=$(system_profiler SPHardwareDataType | awk '/Serial/ {print $4}')

############################################################
# Obtain Computer Device Name from JAMF
############################################################
COMPUTERNAME=$(curl -H "Accept: text/xml" -sfku "${apiuser}:${apipass}" "${jssURL}/JSSResource/computers/serialnumber/${SERIAL}/subset/general" | xmllint --format - 2>/dev/null | awk -F'>|<' '/<name>/{print $3; exit}')

############################################################
#API Group ID , NAME & URL Identifiers for DDPE_UnEncrypted
############################################################
GROUP_ID="227"
GROUP_NAME="DDPE_UnEncrypted"
URL="JSSResource/computergroups/id/${GROUP_ID}"

############################################################
#XML header information
############################################################
xmlHeader="<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"no\"?>"

############################################################
#Add Computer to DDPE_UnEncrypted Static Group
############################################################
Data="<computer_group><id>${GROUP_ID}</id><name>${GROUP_NAME}</name><computer_additions><computer><name>${COMPUTERNAME}</name></computer></computer_additions></computer_group>"


curl -sSkiu ${apiuser}:${apipass} "${jssURL}/${URL}" \
	-H "Content-Type: text/xml" \
	-d "${xmlHeader}${Data}" \
	-X PUT  > /dev/null

############################################################
# Force Jamf Recon
#############################################################
sudo jamf recon
