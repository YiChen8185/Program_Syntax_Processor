# Project-Program_Syntax_Processor

Overview

The goal of this project is to write an interpreter for a simple functional language called PLAN. The interpreter itself should be written in Scheme. A PLAN program is a list and defined by the following grammar:

⟨P rogram⟩ ⟨Expr⟩ ::= ( prog ⟨Expr⟩ ) 

​					⟨Expr⟩ ::= ⟨I d⟩

​									| ⟨Const⟩
 									| ( myif ⟨Expr⟩ ⟨Expr⟩ ⟨Expr⟩)
 									| ( myadd ⟨Expr⟩ ⟨Expr⟩ )
​									 | ( mymul ⟨Expr⟩ ⟨Expr⟩ )
​									| ( mysub ⟨Expr⟩ ⟨Expr⟩ )
​									| ( mylet ⟨Id⟩ ⟨Expr⟩ ⟨Expr⟩ )
​									 | ( mylet ⟨Id⟩ ( myfunction ⟨Id⟩ ⟨Expr⟩ ) ⟨Expr⟩ ) | ( ⟨Id⟩ ⟨Expr⟩ )

​						<id> 	 ::=a|b|...|z

​				<const> 	::= integer constant
 As examples, here are five valid PLAN programs

1. (prog 5)
2. (prog (myadd (myadd 7 (myif (mymul 4 5) 0 10)) (mymul 2 5)))
3. (prog (mylet z (myadd 4 5) (mymul z 2)))
4. (prog (mylet a 66 (myadd (mylet b (mymul 2 4) (myadd 2 b)) (mymul 2 a))))
5. (prog (mylet x 66 (myadd (mylet x (mymul 2 4) (myadd 2 x)) (mymul 2 x))))

---

Each PLAN program and expression evaluates to an integer value. The semantics of a program are defined as follows:

1. The entire program (prog ⟨Expr⟩) evaluates to whatever ⟨Expr⟩ evaluates to.

2. (myif ⟨Expr⟩ ⟨Expr⟩ ⟨Expr⟩) evaluates the first expression. If this is non-zero, it evaluate to

   the second expression, otherwise it evaluates to the third expression.

3. (myadd ⟨Expr⟩ ⟨Expr⟩) evaluates to the sum of whatever values the two sub-expression evaluate to.

4. (mymul ⟨Expr⟩ ⟨Expr⟩) evaluates to the product of whatever values the two sub-expression evaluate to.

5. (mysub ⟨Expr⟩ ⟨Expr⟩) evaluates to the difference of whatever values the two sub-expression evaluate to.

6. (mylet ⟨Id⟩ ⟨Expr⟩1 ⟨Expr⟩2) has the following semantics. First, ⟨Expr⟩1 is evaluated. The resulting integer value is bound to the identifier ⟨Id⟩. Then the second sub-expression ⟨Expr⟩2 is evaluated, and the result of that evaluation serves as the value of the entire mylet expression.

   The binding between the id and the integer value is active only while ⟨Expr⟩2 is being evaluated.

7. ⟨Id⟩ evaluates to the value to which the identifier has been bound by a mylet expression. PLAN will use dynamic scoping, i.e. if there are multiple bindings for the identifier the most recently executed active binding is used.

8. ⟨Const⟩ evaluates to the value of the integer constant.

9. The semantics of (mylet ⟨Id⟩ (myfunction ⟨Id⟩ ⟨Expr⟩) ⟨Expr⟩) and (⟨Id⟩ ⟨Expr⟩) are more

   complicated and are discussed in their own section below.