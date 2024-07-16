@IsTest
private class ProductZoningServiceTest {

    @TestSetup
    static void setup() {
        // Create Product
        Product2 product = new Product2(Name = 'Test Product', ProductCode = 'GC1020', Family = 'TestFamily');
        insert product;
    }

    @IsTest
    static void testGetPermissibleFlyZone() {
        // Test with valid product code and country code
        RestRequest req = new RestRequest();
        req.requestURI = '/services/apexrest/ProductZoning/';
        req.addParameter('ProductCode', 'GC1020');
        req.httpMethod = 'GET';
        req.headers.put('CountryCode', 'US');
        RestContext.request = req;
        RestContext.response = new RestResponse();

        String response = ProductZoningService.getPermissibleFlyZone();
        System.assertEquals('Confirm with the local authorities', response);

        // Test with valid product code and different country code
        req.headers.put('CountryCode', 'DE');
        response = ProductZoningService.getPermissibleFlyZone();
        System.assertEquals('Confirm with the local authorities', response);

        // Test with valid product code but country code not defined
        req.headers.put('CountryCode', 'FR');
        response = ProductZoningService.getPermissibleFlyZone();
        System.assertEquals('Confirm with the local authorities', response);

        // Test with missing product code
        req = new RestRequest();
        req.requestURI = '/services/apexrest/ProductZoning/';
        req.httpMethod = 'GET';
        RestContext.request = req;
        response = ProductZoningService.getPermissibleFlyZone();
        System.assertEquals('ProductCode is missing or doesn\'t exist', response);

        // Test with non-existent product code
        req = new RestRequest();
        req.requestURI = '/services/apexrest/ProductZoning/';
        req.addParameter('ProductCode', 'INVALID');
        req.httpMethod = 'GET';
        RestContext.request = req;
        response = ProductZoningService.getPermissibleFlyZone();
        System.assertEquals('ProductCode is missing or doesn\'t exist', response);
    }
}
