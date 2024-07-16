@RestResource(urlMapping='/lost')
global without sharing class AssetService {

    @HttpPost
    global static String reportLostDevice (String assetIdentifier) {

        List<Asset> assets = [SELECT Id, Name, Asset_Identifier__c, Status,ContactId FROM Asset WHERE Asset_Identifier__c = :assetIdentifier];


        if (assets.size() > 0) {

            assets[0].Status = 'Lost';

            update assets[0];

            List<Insurance__c> insurances = [SELECT Id, Asset__c FROM Insurance__c WHERE Asset__c = :assets[0].Id AND Coverage__c ='Comprehensive' AND Active__c = true LIMIT 1];

            if (insurances.size() > 0) {

                List<Claim__c> claims = [SELECT Id,Name FROM Claim__c WHERE Insurance__c = :insurances[0].Id AND Type__c = 'Loss' LIMIT 1];

                if (claims.size() <= 0) {

                    Claim__c claim = new Claim__c();

                    claim.Insurance__c = insurances[0].Id;

                    claim.Status__c = 'New';

                    claim.Contact__c = assets[0].ContactId;

                    claim.Type__c = 'Loss';
                    claim.Asset__c = assets[0].Id;

                    insert claim;

                    Claim__c claimUpdated = [SELECT Id,Name FROM Claim__c WHERE Insurance__c = :insurances[0].Id AND Type__c = 'Loss' Order by CreatedDate DESC LIMIT 1 ];

                    return claimUpdated.Name;

                 }

                 else {

                     return claims[0].Name + ' already filed.';

                 }

                }

                else {

                    return 'No coverage. Asset status adjusted to Lost.';

                }

        }

        //If the asset record is not found, return an error message.

        else {

            return 'No device found.';

        }

    }

 

}