# Server side validation

Just so we can test the server side validation, we're going to update our **Secured Mock API** to include some validation.

{% code-tabs %}
{% code-tabs-item title="auth-json-server.js" %}
```javascript
function validateSuppliers(req, res, next) {
    console.log('validating')
    const { address, companyName, contactName, contactTitle } = req.body
    var errors = {}

    if (!companyName) errors.companyName = 'Required'
    if (!contactName) errors.contactName = 'Required'
    if (!contactTitle) errors.contactTitle = 'Required'
    if (!address) errors.address = 'Required'
    else {
        if (!address.street) errors['address.street'] = 'Required'
        if (!address.city) errors['address.city'] = 'Required'
        if (!address.region) errors['address.region'] = 'Required'
        if (!address.postalCode) errors['address.postalCode'] = 'Required'
        if (!address.country) errors['address.country'] = 'Required'
        else {
            if (address.country.toLowerCase() == 'australia') {
                if (address.postalCode.length != 4) {
                    errors['address.postalCode'] = 'Invalid'
                }
            }
        }
    }
    if (Object.keys(errors).length > 0) {
        res.status(422).json({ errors })
    } else {
        next()
    }
}

server.post('/suppliers', validateSuppliers)
server.put(/^\/suppliers\/.*$/, validateSuppliers)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now let's update the **SupplierEdit.vue** component to start using this server side validation. First we going to update the section where we call the service to update the supplier. We going to catch errors and if the status code is 422 it means validation error then we bind it to an errors variable in the data

{% code-tabs %}
{% code-tabs-item title="SupplierEdit.vue" %}
```javascript
SupplierService.update(this.model)
    .then(() => this.navigateBack())
    .catch(err => {
        if (err.response.status == 422) {
            this.errors = err.response.data.errors
        }
    })
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then in our template, we display the error message according to the field we're validating

{% code-tabs %}
{% code-tabs-item title="SupplierEdit.vue" %}
```markup
<div class="form-group">
    <label class="col-form-label">Company Name</label>
    <input type="text" class="form-control" 
        id="companyNameField" 
        v-model="model.companyName" 
        :class="{ 'is-invalid': errors && errors.companyName }">
    <div class="invalid-feedback" 
        v-if="errors && errors.companyName">{{ errors.companyName }}
    </div>
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

