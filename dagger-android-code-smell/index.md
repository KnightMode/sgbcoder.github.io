# Eliminating code smell with Dagger’s MultiBinding

{{< admonition >}}
This articles assumes the reader has basic knowledge on Dagger and Android Components.
{{< /admonition >}}

Dagger is a vast universe and learning something new every day motivates me to revisit my code and think of ways to refactor it and make it clean.

One such effort is to exploit Dagger’s Multi-binding feature to refactor the typical ‘View Holder’ class in Android.

<!--more-->

## 1 Problem Statement:

Let’s take a hypothetical scenario where we have a social media aggregator app which can hold data of various apps like WhatsApp , Facebook, Viber etc.

Assuming the layout shown by each of these apps is different, The layout of the landing page of this app might look something like this:

![Layout](https://miro.medium.com/max/1400/1*O2ou1Ujlw-euytVKaUB_oQ.png)

This is a classic case of the Multiple View-Holder Pattern inside a RecyclerView and the typical ViewHolder inflation logic might look like this(Note: The below code uses DataBinding):

## 2 Conventional RecyclerView Adapter:

<script src="https://gist.github.com/sgbcoder/e7d3582fe97575d1e61c8ede353547c1.js"></script>

From the above code, the first observation would be the ever growing switch case when more social media apps will be supported in the future, secondly we are altering a code which is already tried and tested and is in clear violation of SOLID.

We would not be going too deep explaining code smell but refer this article below for more info on switch statement code smell:

https://www.c-sharpcorner.com/article/switch-statement-a-code-smell/

## 3 So, What’s the way out?

Dagger offers Multibinding as a way out of this and quoting the Google’s definition of Multibinding:

“Dagger allows you to bind several objects into a collection even when the objects are bound in different modules using Multibindings. Dagger assembles the collection so that application code can inject it without depending directly on the individual bindings”

For our scenario,

We would be creating a Map with the Key being the contact-type enum (Namely WhatsApp, Facebook etc) and the value being the corresponding ViewHolderWrapper class.

(Why it’s a wrapper and not the corresponding ViewHolder will be explained ahead)

The @IntoMap Keyword used along with the annotations IntKey, StringKey etc (for the respective datatype of the Value.)

Refer:https://dagger.dev/api/latest/dagger/multibindings/package-summary.html for the Multibinding API list.

## 4 Refactored Code

The code for this Map generation can reside inside the ContactsModule class as below:

<script src="https://gist.github.com/sgbcoder/3ed9dfcc1c90ad85f2cdb72cec4afb9e.js"></script>

From the above code we observe a generic Wrapper class created which would contain the template as shown below:

<script src="https://gist.github.com/sgbcoder/5e76da067964308d9b2af52bb958edca.js"></script>

The Refactored Adapter class now looks like this:

<script src="https://gist.github.com/sgbcoder/ed75bd060ceb7e617be112ff6f6f918a.js"></script>

An alternate approach would be to use AssistedInject (instead of using a Wrapper class as shown above) not covered as part of this article.

Hope the article was useful!.

