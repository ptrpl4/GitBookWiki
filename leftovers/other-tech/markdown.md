# markdown

#### links

- https://doka.guide/tools/markdown/ 

---

## Text

*Can **insert** one *type* into another* (sometimes)

*This text will be italic*  
_This text will be italic_

**Bold**  
__Bold__

***Both***  
___Both___  

### Extended

~~strikethrough~~

>! Spoiler!

---

## headlines

# The Biggest headline, it turns into a  `<h1>`

## headline 2

### headline 3

#### headline 4

#### headline 5 

#### headline 6

Text

---

## Links

- https://hexlet.io — the text of a simple link will become a clickable link automatically
- [Это ссылка на Хекслет](https://hexlet.io)
- simple link <https://githib.com>
- simple mail <hi@mail.bue>

### Reference

У [Доки][1] есть свой [репозиторий][repo].


[1]: https://doka.guide "Энциклопедия про web-dev"
[repo]: https://github.com/doka-guide "Репозиторий Доки"

---

## Quotes

> That's a wise quote.\
> A wise man.

---

> Coffee. The finest organic suspension ever devised... I beat the Borg with it - Captain Janeway

---

> # Quotes from great men
> Your work will fill most of your life and the only way to be\
> fully satisfied is to do what you think is a great job.\
> And the only way to do great things is to love what you do.\
>
> *- Steve Jobs, Stanford Speech.*

---

> The first, parental quote
> > And this is the second.\
> > A child quote

---

## Pics

![file](alt-text)

^ example, without real pic

---

## Code

one line `function(12)` 

```javascript
const func = (num) => {
  if (num > 0) {
    return num - 1;
  }
  return num + 1;
};
```

---

## Lists

### Unordered list

#### v1

`-`

- One
- One
- Onetwoth...
- And one more

#### v2

`*`

* it may be `*`
* ok?
* ok!

#### v3 

`+`

+ one
+ two
+ etc

### Ordered list

1. Item
1. Another paragraph (will be `2.`)
	1. Subparagraph (will be `1.`)
	1. Another subparagraph (will be `2.`)
	1. 3rd (will be `3.`)

### Checkboxes (extended)

- [ ] Do stuff
	- [x] not to die


## Strings

trailing space  
check (two lines expected)

no trailing 
space (one line)

no trailing
space (one line)

backslash\
check (two lines)

br-br?<br>
br!
