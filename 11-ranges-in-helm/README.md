# Ranges in Helm

Use loops and ranges to dynamically generate configuration files using templating. Loops, such as the "for" loop in many programming languages, execute a block of code repetitively by iterating over a collection of data. For example, consider the following simple loop which prints numbers from 1 to 10:

```
for i in 1 to 10:
    print i
end
```

In Helm charts, range is a control structure used in templates to loop over lists, arrays, maps, or other collections in your ```values.yaml``` file or other sources.

