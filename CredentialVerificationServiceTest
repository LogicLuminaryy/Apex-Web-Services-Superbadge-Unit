@isTest
private class CredentialVerificationServiceTest {

    @isTest
    static void testVerifyCredential_Valid() {
        // Create test data
        Contact testContact = new Contact(LastName = 'Doe');
        insert testContact;
		Certification__c cert = new Certification__c(Name='SalesforceCertification');
        insert cert;

        // Create a Certification record
        Date issueDate = Date.today();
        Contact_Certification__c testCertification = new Contact_Certification__c(
            isActive__c = true,
            Certification__c = cert.Id, // Replace with actual Certification Name
            Issue_Date__c = issueDate,
            Contact__c = testContact.Id
        );
        insert testCertification;

        Contact_Certification__c conCert = [Select Name from Contact_Certification__c where contact__c = :testContact.Id];
        Test.startTest();
        String result = CredentialVerificationService.verifyCredential(testContact.LastName, conCert.Name);
        Test.stopTest();

        // Verify the result
        System.assertEquals('Valid', result, 'Expected result to be "Valid"');
    }

    @isTest
    static void testVerifyCredential_InactiveCertification() {
        // Create test data
        Contact testContact = new Contact(LastName = 'Smith');
        insert testContact;
		Certification__c cert = new Certification__c(Name='Pilot Certification');
        insert cert;
        // Create an inactive Certification record
        Date issueDate = Date.today().addYears(-2);
        Contact_Certification__c testCertification = new Contact_Certification__c(
            isActive__c = false,
            Certification__c = cert.Id, // Replace with actual Certification Name
            Issue_Date__c = issueDate,
            Contact__c = testContact.Id
        );
        insert testCertification;
        Contact_Certification__c conCert = [Select Name from Contact_Certification__c where contact__c = :testContact.Id];

        // Call the method to test
        Test.startTest();
        String result = CredentialVerificationService.verifyCredential(testContact.LastName, conCert.Name);
        Test.stopTest();

        // Verify the result
        System.assertEquals('Needs Renewal', result, 'Expected result to be "Needs Renewal"');
    }

    @isTest
    static void testVerifyCredential_NoContact() {
        // Call the method with non-existent contact
        Test.startTest();
        String result = CredentialVerificationService.verifyCredential('NonExistentLastName', 'Some Certification');
        Test.stopTest();

        // Verify the result
        System.assertEquals('No record found', result, 'Expected result to be "No record found"');
    }

    @isTest
    static void testVerifyCredential_NoCertification() {
        // Create test data with a contact but no certification
        Contact testContact = new Contact(LastName = 'Johnson');
        insert testContact;

        // Call the method to test
        Test.startTest();
        String result = CredentialVerificationService.verifyCredential(testContact.LastName, 'NonExistentCertification');
        Test.stopTest();

        // Verify the result
        System.assertEquals('No record found', result, 'Expected result to be "No record found"');
    }
}