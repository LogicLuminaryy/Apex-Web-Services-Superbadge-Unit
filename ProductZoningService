@RestResource(urlMapping='/ProductZoning/*')
global with sharing class ProductZoningService {

    @HttpGet
    global static String getPermissibleFlyZone() {
        RestRequest req = RestContext.request;
        RestResponse res = RestContext.response;

        String countryCode = req.headers.get('CountryCode');
        if (String.isEmpty(countryCode)) {
            countryCode = 'US'; // Default to US if CountryCode header is not provided
        }

        // Retrieve product code from query parameters
        String productCode = req.params.get('ProductCode');

        if (String.isEmpty(productCode)) {
            return 'ProductCode is missing or doesn\'t exist';
        }

        List<Product2> products = [SELECT Family FROM Product2 WHERE ProductCode = :productCode LIMIT 1];

        if (products.isEmpty()) {
            return 'ProductCode is missing or doesn\'t exist';
        }

        Product2 product = products[0];

        List<Product_Geo_Mapping__mdt> mappings = [
            SELECT Permissible_Fly_Zone__c 
            FROM Product_Geo_Mapping__mdt 
            WHERE Product_Family__c = :product.Family 
            AND Country_Code__c = :countryCode 
            LIMIT 1
        ];

        if (!mappings.isEmpty()) {
            return mappings[0].Permissible_Fly_Zone__c;
        } else {
            return 'Confirm with the local authorities';
        }
    }
}
