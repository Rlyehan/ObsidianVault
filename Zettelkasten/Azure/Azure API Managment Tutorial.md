222022-08-12

Style: 

Domain: #Azure

Tags: #API 

# Azure API Managment Tutorial

### Transform and protect your API
There might be headers in our API responsed that we don't wan tto expose to our users. We can make use of the transformation policies to strip the from the header of the response sent out from API managment.

To do so we want to go to API > Design > all operations and add a policy in the outbound processing.

There we select the policy "Set headers" where we can enter the names of headers and select the action delete to remove them form the outgoing response.

We might also want to hide any potential URl references, to do so we would do something like this:
1.  osition the cursor inside the **`<outbound>`** element on a blank line. Then select **Show snippets** at the top-right corner of the screen.
    
    ![Select show snippets](https://docs.microsoft.com/en-us/azure/api-management/media/transform-api/show-snippets-1.png)
    
2.  In the right window, under **Transformation policies**, select **Mask URLs in content**.
    
    The **`<redirect-content-urls />`** element is added at the cursor.
    
    ![Mask URLs in content](https://docs.microsoft.com/en-us/azure/api-management/media/transform-api/mask-urls-new.png)
    
3.  Select **Save**.

### Debugging your API

The best way to debug our API ist to make use of tracing. To do so we need to enable tracing on the subscription level. Note that we only want tracing for debuging and development purposes, not for an production API.



___
# References
