
## Chapter 2, day 1
***1. Deploy a contract to account 0x03 called "JacobTucker". Inside that contract, declare a constant variable named is, and make it have type String. Initialize it to "the best" when your contract gets deployed.***

Saved playground - https://play.onflow.org/d3a917fc-bb3f-4f74-8f6a-76d91aa0af70

***2. Check that your variable is actually equals "the best" by executing a script to read that variable. Include a screenshot of the output.***
![Screen Shot 2022-09-11 at 1 04 51 PM](https://user-images.githubusercontent.com/666455/189540047-e7a96dd5-0aa0-4085-947c-8e7fc4ab8c2f.png)


## Chapter 2, day 2


***1. Explain why we wouldn't call changeGreeting in a script.***
Scripts can only be used to read data and can't write. Transactions are used to write data

***2. What does the AuthAccount mean in the prepare phase of the transaction?***
Its the type of the variable.  AuthAcccount is the authorized account (or user) signing the contract.

***3. What is the difference between the prepare phase and the execute phase in the transaction?***
Prepare block gives you access to information in the signer's account. Its safer/clearer to do logic in execute if its not dependent on signer data.

***4. This is the hardest quest so far, so if it takes you some time, do not worry! I can help you in the Discord if you have questions.***

- Add two new things inside your contract:
  - A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
  - A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber
- Add a script that reads myNumber from the contract
- Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

https://play.onflow.org/d3a917fc-bb3f-4f74-8f6a-76d91aa0af70

## Chapter 2, Day 3

***1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.***
```
pub fun main(): Int {
  var people: [String] = ["Jacob", "Alice", "Damian"]
  log(people)
}
```

***2. In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!***
```cadence
pub fun main(): Int {
  var socialMedia: { String: UInt64 } = { "YouTube" : 3, "Reddit": 1, "Facebook": 4, "Instagram": 2 }
  return 0
}
```

***3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).***
```cadence
pub fun main(): Int {
  var cats: { String: Int } = {"Felix": 1, "Garfield": 2}
  return cats["Felix"]!
}
```
Force unwrap unwraps an optional to its type.  Its saying that there must be a value that is not `nil` otherwise it will PANIC. Dictionary values always return optionals.

***4. Using this picture below, explain...***
 - ***What the error message means***: Its returning an optional and the function expected return type is a string
 - ***Why we're getting this error***: because dictionaries return optinal values
 - ***How to fix it***: Either force unwrap it (`thing[0x03]!`) or change the return type to be an optional (`pub fun main(): String? {`)  

![img](https://github.com/emerald-dao/beginner-cadence-course/raw/main/chapter2.0/images/wrongcode.png)

## Chapter 2, Day 4


1. Deploy a new contract that has a Struct of your choosing inside of it (must be different than Profile).
```cadence
pub contract Authentication {

    pub var cats: [Cat]
    
    pub struct Cat {
        pub let name: String
        pub let account: Address

        // You have to pass in 4 arguments when creating this Struct.
        init(_name: String, _account: Address) {
            self.name = _name
            self.account = _account

        }
    }

    pub fun addCat(name: String, account: Address) {
        let newCat = Cat(_name: name, _account: account)
        self.cats.append(newCat)
    }

    init() {
        self.cats = []
    }

}
```
2. Create a dictionary or array that contains the Struct you defined.
see 1
3. Create a function to add to that array/dictionary.
see 1
4. Add a transaction to call that function in step 3.
```cadence
import Authentication from 0x01

transaction(name: String) {
    let addr: Address

    prepare(signer: AuthAccount) {
        self.addr = signer.address
    }

    execute {
        Authentication.addCat(name: name, account: self.addr)
        log("catn added.")
    }
}
```
5. Add a script to read the Struct you defined.
```cadence
import Authentication from 0x01

pub fun main(index: Int): Authentication.Cat {
    return Authentication.cats[index]!
}
```

https://play.onflow.org/3540b9cb-3ab8-4097-9ed4-b20da2f53133?type=script&id=c6671860-214f-4ea3-a1c5-17e762419b4e&storage=none
