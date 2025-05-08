here write blog ...
TypeScript-এ, interface ও type উভয়কেই কাস্টম টাইপ নির্ধারণে ব্যবহার করা হয় এবং এদের অনেক মিল রয়েছে।

১. Extending ও Implementing
ইন্টারফেস ও টাইপের মধ্যে একটি বড় পার্থক্য হলো কীভাবে তারা উত্তরাধিকার (inheritance) পরিচালনা করে।

Interfaces: extends কীওয়ার্ড ব্যবহার করে একাধিক ইন্টারফেস এক্সটেন্ড করতে পারে এবং classes ইন্টারফেস implement করতে পারে।
interface Animal {
  name: string;
  age: number;
}

interface Dog extends Animal {
  breed: string;
}

class Labrador implements Dog {
  name: string;
  age: number;
  breed: string;

  constructor(name: string, age: number, breed: string) {
    this.name = name;
    this.age = age;
    this.breed = breed;
  }
}

Types: & (intersection) বা | (union) অপারেটর ব্যবহার করে টাইপ কম্বাইন করা যায়, কিন্তু extends ব্যবহার করা যায় না।
type Animal = {
  name: string;
  age: number;
};

type Dog = Animal & {
  breed: string;
};

 ঘোষণা মার্জিং (Declaration Merging)
Interfaces-এর একটি শক্তিশালী ফিচার হলো ঘোষণা মার্জিং। একই ইন্টারফেস একাধিকবার ঘোষণা করলে TypeScript সেগুলো একত্র করে ফেলে।
interface Person {
  name: string;
}

interface Person {
  age: number;
}

const john: Person = {
  name: "John",
  age: 30,
};
Person ইন্টারফেস দুইবার ঘোষণা করা হলেও TypeScript সেগুলো মিশিয়ে একটি ইন্টারফেসে পরিণত করে।

Types-এর ক্ষেত্রে এটি সম্ভব নয় — একই টাইপ আবার ঘোষণা করলে TypeScript ত্রুটি দেখায়।

ব্যবহারের ক্ষেত্র ও নমনীয়তা
Interfaces ক্লাস বা অবজেক্টের কাঠামো নির্ধারণে ভালো, বিশেষ করে অবজেক্ট ওরিয়েন্টেড প্রোগ্রামিংয়ে।

Types আরও নমনীয়, যেমন: union, intersection, বা conditional টাইপের মতো জটিল টাইপ সংজ্ঞায়নে ব্যবহার করা যায়।

==>keyof অপারেটর অবজেক্ট টাইপ থেকে সব কী বের করে একটি ইউনিয়ন টাইপ তৈরি করে।
interface Person {
  name: string;
  age: number;
  address: string;
}

type PersonKeys = keyof Person;
// ফলাফল: "name" | "age" | "address"
keyof ব্যবহার করে আপনি অবজেক্টের কী নিয়ে আরও টাইপ-সেফ কোড লিখতে পারেন — বিশেষ করে যখন কী ডাইনামিকভাবে অ্যাক্সেস করা হয়।
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const person = {
  name: "Alice",
  age: 30,
  address: "123 Main St",
};

const name = getProperty(person, "name"); // "Alice"
const age = getProperty(person, "age");   // 30
এখানে K extends keyof T টাইপ কনস্ট্রেইন্ট ব্যবহার করে নিশ্চিত করা হয়েছে যে key-টি অবশ্যই obj-এর একটি বৈধ কী হতে হবে।
keyof-কে Mapped Type-এর সঙ্গে ব্যবহার করে আপনি টাইপ ট্রান্সফরমেশন করতে পারেন।
type Partial<T> = {
  [K in keyof T]?: T[K];
};

interface Person {
  name: string;
  age: number;
  address: string;
}

const partialPerson: Partial<Person> = {
  name: "Bob",
};