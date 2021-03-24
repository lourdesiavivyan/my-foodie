<template>
    <div id="rest-signup">
        <h1>Sign up to start taking orders!</h1>
        <form id="signup">
            <h2>Account Information</h2>
            <label>First Name: </label>
            <input type="text" v-model="restaurant.first_name" required /><br><br>
            <label>Last Name: </label>
            <input type="text" v-model="restaurant.last_name" required /><br><br>
            <label>Restaurant Name: </label>
            <input type="text" v-model="restaurant.restaurant_name" required /><br><br>
            <label title="Create a unique username to log in">Username: </label>
            <input type="text" v-model="restaurant.username" title="Create a unique username to log in" required /><br><br>
            <label>Password: </label>
            <input type="password" v-model="restaurant.password" required /><br><br>
            <label>Confirm Password: </label>
            <input type="password" v-model="cfmPwd" v-on:keyup="checkPassword()" required /><br>
            {{pwdText}}<br>
            <label>Email Address (admin): </label>
            <input type="email" v-model="restaurant.email" required /><br><br>
            <label>Email Address (customer service): </label>
            <input type="email" v-model="restaurant.contact_email" required /><br><br>
            <label>Contact Number: </label>
            +65 <input type="tel" v-model="restaurant.contact_num" pattern="[0-9]{8}" required /><br><br>
            <label>Address: </label><br>
            <textarea v-model="restaurant.address" cols="30" rows="2" required /><br><br>
            <label>Postal Code: </label>
            <input type="tel" v-model="restaurant.postal_code" pattern="[0-9]{6}" required /><br><br>
            <h2>Payment Details</h2>
            Currently, we only accept payment by credit/debit card.<br><br>
            <label>Card Number: </label>
            <!-- <input type="text" v-model="restaurant.cardNum" required /><br><br> -->
            <input type="text" v-model="restaurant.card.number" required /><br><br>
            <label>Name on card: </label>
            <input type="text" v-model="restaurant.card.name" required /><br><br>
            <label>CVV: </label>
            <input type="tel" v-model="restaurant.card.cvv" pattern="[0-9]{3}" required /><br><br>
            <label>Expiry Date: </label>
            <input type="month" v-model="restaurant.card.expiry" placeholder="MM/YY" required /><br><br>
            <button type="submit" v-on:click.prevent="createRest()">Create my account!</button>
        </form>
    </div>
</template>


<script>
import database from '../firebase.js'

export default {
    data() {
        return {
            cfmPwd: "",
            pwdText: "",
            restaurant: {
                username: "",
                restaurant_name: "",
                first_name: "",
                last_name: "",
                password: "",
                email: "",
                contact_email: "",
                contact_num: "",
                address: "",
                postal_code: "",

            }
        }
    },
    methods: {
        checkPassword() {
            if (this.restaurant.password!="") {
                if (this.cfmPwd === this.restaurant.password) {
                    this.pwdText = ""
                } else {
                    this.pwdText = "Your password does not match"
                }
            } else {
                this.pwdText = ""
            }
        },
        createRest() {
            // add restaurant details to firebase
            let toCreate = true;

            // check if account exists
            database.collection('restaurants').get().then(snapshot => {
                snapshot.docs.forEach(doc => {
                    if (doc.id === this.restaurant.username) {
                        alert("Username exists, please try another username.");
                        toCreate = false;
                    }
                })
            }).then(() => {
                if (toCreate === true) {
                    database.collection('restaurants').doc(this.restaurant.username).set(this.restaurant);
                    alert("Account created successfully!")
                    // location.reload();
                    // push to login page
                }
            })
        }
    }
}
</script>


<style scoped>
#cust-signup {
    font-family: Poppins;
    font-style: normal;
    /* text-align: left; */
    /* padding-left: 300px; */
}
</style>