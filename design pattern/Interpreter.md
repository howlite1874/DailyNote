```c++
#include <iostream>
#include <memory>

class Context {};

class AbstractExpression {
public:
    virtual int interpret(Context& context) const = 0;
    virtual ~AbstractExpression() = default;
};

class TerminalExpression : public AbstractExpression {
public:
    TerminalExpression(int value) : value(value) {}

    int interpret(Context& context) const override {
        return value;
    }

private:
    int value;
};

class NonterminalExpression : public AbstractExpression {
public:
    NonterminalExpression(std::unique_ptr<AbstractExpression> left, std::unique_ptr<AbstractExpression> right, char op)
        : left(std::move(left)), right(std::move(right)), op(op) {}

    int interpret(Context& context) const override {
        if (op == '+') {
            return left->interpret(context) + right->interpret(context);
        } else if (op == '-') {
            return left->interpret(context) - right->interpret(context);
        }
        return 0;
    }

private:
    std::unique_ptr<AbstractExpression> left, right;
    char op;
};

int main() {
    Context context;

    auto leftExpression = std::make_unique<TerminalExpression>(5);
    auto rightExpression = std::make_unique<TerminalExpression>(3);
    auto expression = std::make_unique<NonterminalExpression>(std::move(leftExpression), std::move(rightExpression), '+');

    std::cout << "Result: " << expression->interpret(context) << '\n';

    return 0;
}

```
