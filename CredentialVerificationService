global class CredentialVerificationService {

    webservice static String verifyCredential(String lastName, String certificationName) {
        System.debug('Input Parameters - LastName: ' + lastName + ', CertificationName: ' + certificationName);

        String response = 'No record found';

        // Query Contact based on LastName
        List<Contact> contacts = [SELECT Id FROM Contact WHERE LastName = :lastName LIMIT 1];
        if (!contacts.isEmpty()) {
            Contact contact = contacts[0];

            // Query Contact Certification based on CertificationName and Contact Id
            List<Contact_Certification__c> certifications = [
                SELECT Id, isActive__c 
                FROM Contact_Certification__c 
                WHERE Name = :certificationName AND Contact__c = :contact.Id 
                LIMIT 1
            ];

            if (!certifications.isEmpty()) {
                Contact_Certification__c certification = certifications[0];
                if (certification.isActive__c) {
                    response = 'Valid';
                } else {
                    response = 'Needs Renewal';
                }
            }
        }

        System.debug('Response: ' + response);
        return response;
    }
}